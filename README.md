When I am hitting this in DEV env, I am getting 400 BAD_Request
https://jci44nmbpa-vpce-07100da6fbb2a281c.execute-api.us-east-1.amazonaws.com/dev/sorting-service/api/v1/schedules/"38"/disposition-lots/CVS252860032

Response
{

"error": {

"code": "INVALID_REQUEST",

"message": "Invalid value '"38"' for parameter 'scheduleId'. Expected type: class java.lang.Integer",

"timestamp": "2025-10-30T11:28:54.709"

}

}

but when I am hitting this in test env , I am gitting 500 Internal Server error
https://jci44nmbpa-vpce-07100da6fbb2a281c.execute-api.us-east-1.amazonaws.com/test/sorting-service/api/v1/schedules/"38"/disposition-lots/CVS252860032

Respsone

{

"code": "UNEXPECTED_ERROR",

"message": "Method parameter 'scheduleId': Failed to convert value of type 'java.lang.String' to required type 'java.lang.Integer'; For input string: ""38""",

"timestamp": "2025-10-30T11:32:43.948414736"

}

I want to 400 bad request in Test env too instead of 500
