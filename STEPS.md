##### **STEP 1**

* GET - http://localhost:8080/api/v1
* A JSON response containing metadata like the version ("v1") and links to your primary collections (rooms and sensors). This demonstrates HATEOAS for your report.



##### **STEP 2**

* ***Create a Room***
* POST - http://localhost:8080/api/v1/rooms
* Body - {"id": "LIB-301", "name": "Library Quiet Study", "capacity": 50}



* GET - http://localhost:8080/api/v1/rooms/LIB-301
* The full JSON object of the room you just created.



##### **STEP 3**

* ***Add a Sensor***
* POST - http://localhost:8080/api/v1/sensors
* Body - {"id": "TEMP-001", "type": "Temperature", "status": "ACTIVE", "roomId": "LIB-301"}



* ***Add a sensor with a invalid roomId.***
* POST - http://localhost:8080/api/v1/sensors
* Body - {"id": "TEMP-001", "type": "Temperature", "status": "ACTIVE", "roomId": "NON-EXISTENT"}
* Show Postman receiving a 422 Unprocessable Entity (or 400).



* GET - http://localhost:8080/api/v1/sensors?type=Temperature
* Show that only sensors matching that type are returned.



##### **STEP 4**

* ***Update the currentValue.***
* POST - http://localhost:8080/api/v1/TEMP-001/read
* Body - {"value": 24.5}
* GET - http://localhost:8080/api/v1/TEMP-001
* show that currentValue has been updated to 24.5



* ***Update the sensor status to "MAINTENANCE".***
* PUT - http://localhost:8080/api/v1/sensors/TEMP-001
* Headers - Content-Type to application/json
* Body - {"id": "TEMP-001", "type": "Temperature", "status": "MAINTENANCE", "currentValue": 24.5, "roomId": "LIB-301"}
* ***Add a reading to the "MAINTENANCE" sensor.***
* POST - http://localhost:8080/api/v1/TEMP-001/read
* Body - {"value": 26.5}
* Show a 403 Forbidden status in Postman.



##### **STEP 5**

* ***Return the sensor status to "ACTIVE".***
* PUT - http://localhost:8080/api/v1/sensors/TEMP-001
* Headers - Content-Type to application/json
* Body - {"id": "TEMP-001", "type": "Temperature", "status": "ACTIVE", "currentValue": 24.5, "roomId": "LIB-301"}



* ***Delete the room with the ACTIVE sensor.***
* DELETE - http://localhost:8080/api/v1/rooms/LIB-301
* Show a 409 Conflict with your custom JSON error message.



* ***Delete the sensor first and then delete the room.***
* DELETE - http://localhost:8080/api/v1/sensors/TEMP-001
* DELETE - http://localhost:8080/api/v1/rooms/LIB-301
* Show a successful 204 No Content or 200 OK.

