/**
     * Gets the disposition lot status including units and volume.
     *
     * <p>For Production or Further Test lot types:
     * Fetches data from DSP_RESULT table where IS_ACTIVE = true, IS_MISSING = false, and IS_CONFIRMED = true
     *
     * <p>For Destroy lot type:
     * Fetches data from DSP_RESULT (IS_ACTIVE = true, IS_MISSING = false) and
     * DSP_EXTRA_UNIT (IS_ACTIVE = true), then combines both results
     *
     * @param dispositionLotId the ID of the disposition lot
     * @param lotType the type of lot (Production, Further Test, or Destroy)
     * @param siteId the site ID from header (for logging/audit purposes)
     * @return DispositionLotStatusResponse containing units and volume
     * @throws InvalidRequestException if lotType is invalid
     * @throws ResourceNotFoundException if disposition lot is not found
     */
    public DispositionLotStatusResponse getDispositionLotStatus(
            Integer dispositionLotId,
            String lotType,
            String siteId) {

        log.info("Fetching disposition lot status for lotId={}, lotType={}, siteId={}",
                dispositionLotId, lotType, siteId);

        // Validate lotType
        LotType validatedLotType;
        try {
            validatedLotType = LotType.valueOf(lotType.toUpperCase().replace(" ", "_"));
        } catch (IllegalArgumentException e) {
            throw new InvalidRequestException("Invalid lotType: " + lotType
                    + ". Allowed values are: Production, Further Test, Destroy");
        }

        // Validate disposition lot exists
        lotRepository.findByDspLotId(dispositionLotId)
                .orElseThrow(() -> {
                    log.error("Disposition lot not found with ID: {}", dispositionLotId);
                    return new ResourceNotFoundException("Disposition lot not found with ID: " + dispositionLotId);
                });

        Integer totalUnits;
        Float totalVolume;

        if (validatedLotType == LotType.PRODUCTION || validatedLotType == LotType.FURTHER_TEST) {
            // For Production or Further Test: fetch from DSP_RESULT with IS_CONFIRMED = true
            Object[] result = resultRepository.calculateUnitsAndVolumeForProductionOrFurtherTest(dispositionLotId);

            if (result != null && result.length == 2) {
                totalUnits = convertToInteger(result[0]);
                totalVolume = convertToFloat(result[1]);
            } else {
                log.warn("No data found for disposition lot ID: {}", dispositionLotId);
                totalUnits = 0;
                totalVolume = 0.0f;
            }

            log.info("Production/Further Test lot status - units: {}, volume: {}", totalUnits, totalVolume);

        } else if (validatedLotType == LotType.DESTROY) {
            // For Destroy: fetch from both DSP_RESULT and DSP_EXTRA_UNIT
            Object[] resultData = resultRepository.calculateUnitsAndVolumeForDestroy(dispositionLotId);
            Long extraUnitsCount = dspExtraUnitRepository.countActiveExtraUnitsForDestroy(dispositionLotId);

            Integer resultUnits = 0;
            Float resultVolume = 0.0f;

            if (resultData != null && resultData.length == 2) {
                resultUnits = convertToInteger(resultData[0]);
                resultVolume = convertToFloat(resultData[1]);
            } else {
                log.warn("No data found in DSP_RESULT for disposition lot ID: {}", dispositionLotId);
            }

            Integer extraUnits = extraUnitsCount != null ? extraUnitsCount.intValue() : 0;

            totalUnits = resultUnits + extraUnits;
            totalVolume = resultVolume; // Extra units don't have volume

            log.info("Destroy lot status - DSP_RESULT units: {}, volume: {}, DSP_EXTRA_UNIT units: {}, total units: {}",
                    resultUnits, resultVolume, extraUnits, totalUnits);

        } else {
            throw new InvalidRequestException("Unsupported lotType: " + lotType);
        }

        return new DispositionLotStatusResponse(totalUnits, totalVolume);
    }

    /**
     * Converts database result to Integer safely.
     *
     * @param value the value from database query result
     * @return Integer value or 0 if null
     */
    private Integer convertToInteger(Object value) {
        if (value == null) {
            return 0;
        }
        if (value instanceof Integer) {
            return (Integer) value;
        }
        if (value instanceof Long) {
            return ((Long) value).intValue();
        }
        if (value instanceof BigInteger) {
            return ((BigInteger) value).intValue();
        }
        if (value instanceof BigDecimal) {
            return ((BigDecimal) value).intValue();
        }
        if (value instanceof Number) {
            return ((Number) value).intValue();
        }
        try {
            return Integer.parseInt(value.toString());
        } catch (NumberFormatException e) {
            log.warn("Unable to convert value to Integer: {}, returning 0", value);
            return 0;
        }
    }

    /**
     * Converts database result to Float safely.
     *
     * @param value the value from database query result
     * @return Float value or 0.0f if null
     */
    private Float convertToFloat(Object value) {
        if (value == null) {
            return 0.0f;
        }
        if (value instanceof Float) {
            return (Float) value;
        }
        if (value instanceof Double) {
            return ((Double) value).floatValue();
        }
        if (value instanceof BigDecimal) {
            return ((BigDecimal) value).floatValue();
        }
        if (value instanceof Number) {
            return ((Number) value).floatValue();
        }
        try {
            return Float.parseFloat(value.toString());
        } catch (NumberFormatException e) {
            log.warn("Unable to convert value to Float: {}, returning 0.0f", value);
            return 0.0f;
        }
    }
