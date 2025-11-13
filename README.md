Implement a PATCH API to close a Disposition Schedule.

REQUEST :-

Endpoint: GET /v1/schedules/{scheduleId}/close

Path Parameters:

scheduleId  integer  Required

Header:

x-site-id

x-action-timestamp

username



RESPONSE :-

Status : 204 No Content



Implementation Details - Upon receiving the request validate if the schedule is in ACTIVE status. If the schedule is in ACTIVE status update the current status to deactivate it and add a new status as COMPLETED & display status remains as Active. In the DSP_SCHEDULE_SESSION table stop the current session and toggle the active flag to false.

Disposition Lots -

Check if all the Production Disposition Lots in the schedule has the current active status as MOVED_TO_FREEZER.

If they are then deactivate the current MOVED_TO_FREEZER status.

Add a new status for them with the status code MANUAL_SORTED and mark this as currently active.

Disposition Schedule Session -

In the DSP_SCHEDULE_SESSION table update the record which is marked with IS_ACTIVE as true with the following information

SESSION_STOPPED_BY: username

UPDATED_BY: username

SESSION_STOPPED_AT: x-action-timestamp

UPDATED_AT: x-action-timestamp

IS_ACTIVE: false

Disposition Schedule -

Validate if the current DSP_SCHEDULE_STATUS with the IS_ACTIVE = true has the STATUS_CODE as STARTED

deactivate the current status and then add a new status with status code as COMPLETED and mark this as active.

Disposition Schedule Details -

Insert records in DSP_SCHEDULE_DETAILS for each UNIT_TYPE from the DSP_RESULT table where IS_PLANNED = false.

