2025-05-27T11:46:38.271+05:30  INFO 12160 --- [eh-usage-integration] [           main] c.e.u.i.controller.UsageController       : UsageController - Inside UsageDetails

MockHttpServletRequest:
      HTTP Method = POST
      Request URI = /v1/record-ingestion
       Parameters = {}
          Headers = [Content-Type:"application/json;charset=UTF-8", Content-Length:"237"]
             Body = [{"platformId":"platform1","uom":"uom1","quantity":"10","startDate":"2024-06-01","endDate":null,"product":null,"subscriptionId":null,"chargeId":"charge1","description":null,"uniqueKey":"key1","userName":null,"accountNumber":"platform1"}]
    Session Attrs = {org.springframework.security.web.csrf.HttpSessionCsrfTokenRepository.CSRF_TOKEN=org.springframework.security.web.csrf.DefaultCsrfToken@4f2d8175}

Handler:
             Type = null

Async:
    Async started = false
     Async result = null

Resolved Exception:
             Type = null

ModelAndView:
        View name = null
             View = null
            Model = null

FlashMap:
       Attributes = null

MockHttpServletResponse:
           Status = 403
    Error message = Forbidden
          Headers = [X-Content-Type-Options:"nosniff", X-XSS-Protection:"0", Cache-Control:"no-cache, no-store, max-age=0, must-revalidate", Pragma:"no-cache", Expires:"0", X-Frame-Options:"DENY"]
     Content type = null
             Body = 
    Forwarded URL = null
   Redirected URL = null
          Cookies = []

java.lang.AssertionError: Status expected:<200> but was:<403>
Expected :200
Actual   :403
<Click to see difference>


	at org.springframework.test.util.AssertionErrors.fail(AssertionErrors.java:61)
	at org.springframework.test.util.AssertionErrors.assertEquals(AssertionErrors.java:128)
	at org.springframework.test.web.servlet.result.StatusResultMatchers.lambda$matcher$9(StatusResultMatchers.java:640)
	at org.springframework.test.web.servlet.MockMvc$1.andExpect(MockMvc.java:214)
	at com.everhealth.usage.integration.ControllerTest.UsageTestController.testUsageDetails_Success(UsageTestController.java:124)
	at java.base/java.lang.reflect.Method.invoke(Method.java:580)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
	at java.base/java.util.ArrayList.forEach(ArrayList.java:1596)
