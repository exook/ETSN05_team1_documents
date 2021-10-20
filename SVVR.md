# SVVR - Software Verification and Validation Report

# Contents

- [SVVR - Software Verification and Validation Report](#svvr---software-verification-and-validation-report)
- [Contents](#contents)
- [Document history](#document-history)
- [1. Introduction](#1-introduction)
- [2. Tests](#2-tests)
  - [2.1 Area](#21-area)
    - [2.1.1 `AreaDataAccess.java`](#211-areadataaccessjava)
    - [2.1.2 `AreaResource.java`](#212-arearesourcejava)
  - [2.2 Match](#22-match)
    - [2.2.1 `MatchDataAccess.java`](#221-matchdataaccessjava)
    - [2.2.2 `MatchResource.java`](#222-matchresourcejava)
  - [2.3 Route](#23-route)
    - [2.3.1 `DriverRouteDataAccess.java`](#231-driverroutedataaccessjava)
    - [2.3.2 `DriverRouteResource.java`](#232-driverrouteresourcejava)
    - [2.3.3 `PassengerRouteDataAccess.java`](#233-passengerroutedataaccessjava)
    - [2.3.4 `PassengerRouteResource.java`](#234-passengerrouteresourcejava)
  - [2.4 Vehicle](#24-vehicle)
    - [2.4.1 `VehicleDataAccess.java`](#241-vehicledataaccessjava)
    - [2.4.2 `VehicleResource.java`](#242-vehicleresourcejava)
  - [2.5 User](#25-user)
    - [2.5.1 `UserDataAccess.java`](#251-userdataaccessjava)
    - [2.5.2 `UserResource.java`](#252-userresourcejava)
  - [2.6 Frontend Jasmine](#26-frontend-jasmine)
    - [2.6.1 `add-vehicle.js`](#261-add-vehiclejs)
    - [2.6.2 `register.js`](#262-registerjs)

# Document history

| Version | Date | Responsible | Description |
|:-|:-|:-|:-
| 1.0 | 2021-10-14 | SG | Initial draft
| 1.1 | 2021-10-15 | SG | Add test cases for `AreaResource` and `MatchDataAccess`
| 1.2 | 2021-10-15 | SG | Replace "Location" -> "Area" according to recent changes
| 1.3 | 2021-10-18 | SG | Improve formatting and descriptions
| 1.4 | 2021-10-18 | SG | Add test cases for `DriverRouteDataAccess` and `DriverRouteResource`
| 1.5 | 2021-10-18 | SG | Add test cases for `VehicleDataAccess` and `VehicleResource`
| 1.6 | 2021-10-18 | SG | Add outline for remaining tests
| 1.7 | 2021-10-18 | SG | Add test cases for `UserDataAccess`
| 1.8 | 2021-10-18 | SG | Add test cases for `PassengerDataAccess` and `PassengerResource`
| 1.9 | 2021-10-20 | SG | Add jasmine test cases
| 1.10 | 2021-10-20 | SG | Add test cases for `MatchResource` and part of `UserResource`
| 1.11 | 2021-10-20 | SG | Add introduction.

# 1. Introduction

In this document all tests that we have implemented are outlined. Each class is associated with a table showing which methods are being tested, what the test asserts, the arguments we provide to the respective methods, and whether the test passes as of writing this document.

# 2. Tests

## 2.1 Area
### 2.1.1 `AreaDataAccess.java`

| Methods | Test | Description | Arguments | Result |
|:-|:-|:-|:-|:-
| `addArea` & `getArea` | `getAreaAfterAdd` | Inserts an area and asserts that we successfully retrieve the same area. | `"Test", 55.8013888888889, 13.3336111111111` | Pass
| `addArea` & `getAllAreas` | `getAllAreasAfterAdd` | Inserts an area and asserts that we successfully retrieve a list of areas containing it. | `"Test", 55.8013888888889, 13.3336111111111` | Pass
| `getAllAreas` | `getAllAreasNotEmpty` | Asserts that the retrieved list of areas contains elements (should be pre-populated from market research). | | Pass

### 2.1.2 `AreaResource.java`

| Methods | Test | Description | Json / Query Params | Result |
|:-|:-|:-|:-|:-
| `getArea` | `getAreaAsNoOne` | Asserts that no exception is unhandled if we try to get an area without being logged in (ID 1 should be pre-populated from market research). | `1` | Pass
| `getAllAreas` | `getAllAreasAsNoOne` | Asserts that no exception is unhandled if we try to get all areas without being logged in. | | Pass
| `addArea` | `addAreaAsNoOne` | Asserts that we receive a `ForbiddenException` if we try to add an area without being logged in. | `{"name":"Test"}` | Pass
| `addArea` & `getArea` | `getAreaAfterAdd` | Inserts an area and asserts that we successfully retrieve the same area. | `{"name":"Test", "latitude":"55.8013888888889", "longitude":"13.3336111111111"}` & `insertedArea.getId()` | Pass
| `addArea` & `getAllAreas` | `getAllAreasAfterAdd` | Inserts an area and asserts that we successfully retrieve a list of areas containing it. | `{"name":"Test", "latitude":"55.8013888888889", "longitude":"13.3336111111111"}` | Pass
| `getAllAreas` | `getAllAreasNotEmpty` | Asserts that the retrieved list of areas contains elements (should be pre-populated from market research). | | Pass

## 2.2 Match
### 2.2.1 `MatchDataAccess.java`

| Methods | Test | Description | Arguments | Result |
|:-|:-|:-|:-|:-
| `addMatch` & `addMatch` & `getMatch` | `getMatchAfterAdd` | Inserts two matches and asserts that the second is retrieved given its IDs. | `driverRoute1.getId(), passengerRoute1.getId()` & `driverRoute1.getId(), passengerRoute2.getId()` & `driverRoute1.getId(), passengerRoute1.getId()` | Pass
| `addMatch` & `getAllMatches` | `getAllMatchesAfterAdd` | Inserts a match and asserts that we successfully retrieve a list of matches containing it. | `driverRoute.getId(), passengerRoute.getId()` | Pass
| `addMatch` & `getMatchWithPassengerRoute` | `getMatchWithPassengerRoute` | Inserts a match and asserts that we successfully retrieve that match given its passenger route ID. | `driverRoute.getId(), passengerRoute.getId()` & `passengerRoute.getId()` | Pass
| `addMatch` & `addMatch` & `getMatchWithDriverRoute` | `getMatchesWithDriverRoute` | Inserts two matches with the same driver route and asserts that we successfully retrieve both. | `driverRoute1.getId(), passengerRoute1.getId()` & `driverRoute1.getId(), passengerRoute2.getId()` & `driverRoute1.getId()` | Pass
| `addMatch` & `removeMatch` & `getAllMatches` | `removeMatch` | Inserts a match and asserts that removing it is successful and that it is not part of all matches. | `driverRoute.getId(), passengerRoute.getId()` | Pass
| `addMatch` & `updateMatch` & `getMatch` | `updateMatch` | Inserts a match, updates its arrival/departure, and asserts that retrieving the match yields the updated attributes. | `driverRoute.getId(), passengerRoute.getId()` & `driverRoute.getId(), passengerRoute.getId(), new Date(newMatch.getPassengerDepartAt().getTime() + 1000000), new Date(newMatch.getPassengerArriveAt().getTime() + 1000000)` & `driverRoute.getId(), passengerRoute.getId()` | Pass

### 2.2.2 `MatchResource.java`

| Methods | Test | Description | Json / Query Params | Result |
|:-|:-|:-|:-|:-
| `addMatch` & `getAllMatches` | `getAllMatches` | Adds a match to the database and asserts the list of all matches contains it. | `{match: new Match(driver.getId(), passenger.getId, testDate, testDate)}` | Pass
| `addMatch` & `updateMatch` & `getMatchByPassengerRouteId` | `updateMatch` | Adds a match to the database, updates it and asserts it has been updated. | `{match: new Match(driver.getId(), passenger.getId, testDate, testDate)}` & `{match: new Match(driver.getId(), passenger.getId(), addedMatch.getPassengerDepartAt().getTime() + 100000, addedMatch.getPassengerDepartAt().getTime() + 100000)}` & `addedMatch.getPassengerRouteId()` | Pass 
| `removeMatch` | `deleteNonExistingMatch` | Asserts that trying to delete a non existent match throws `NotFoundException`. | `{driverRouteId:-1, passengerRouteId:-1}` | Pass
| `addMatch` & `removeMatch` & `getAllMatches` | `deleteMatch` | Adds a match, deletes it, asserts it was deleted successfully and that it is not in the list of all matches. | `{match: new Match(driver.getId(), passenger.getId, testDate, testDate)}` & `{driverRouteId: addedMatch.getDriverRouteId(), passengerRouteId: addedMatch.getPassengerRouteId()}` | Pass
| `addMatch` & `getMatchByDriverRouteId` | `getMatchesByDriverRouteId` | Adds a match and asserts getting a match with the driver route ID of the added match returns the same match. | `{match: new Match(driver.getId(), passenger.getId, testDate, testDate)}` & `addedMatch.getDriverRouteId()` | Pass 
| `getMatchByDriverRouteId` | `getNonExistingMatchesByDriverRouteId` | Asserts that getting non existent matches by driver route ID returns an empty list. | `"-1"` | Pass 
| `addMatch` & `getMatchByPassengerRouteId` | `getMatchesByPassengerRouteId` | Adds a match and asserts getting a match with the passenger route ID of the added match returns the same match. | `{match: new Match(driver.getId(), passenger.getId, testDate, testDate)}` & `addedMatch.getPassengerRouteId()` | Pass 
| `getMatchByPassengerRouteId` | `getNonExistingMatchByPassengerRouteId` | Asserts that getting non existent match by passenger route ID returns null. | `"-1"` | Pass 


## 2.3 Route
### 2.3.1 `DriverRouteDataAccess.java`

| Methods | Test | Description | Arguments | Result |
|:-|:-|:-|:-|:-
| `addDriverRoute` & `getUserDriverRoutes` | `addDriverRoute` | Adds a driver route and asserts the list of the users driver routes contains it. | `2, testVehicle.getId(), 1, testStartArea.getId(), testEndArea.getId(), testDate, testDate` | Pass |
| `addDriverRoute` | `addNonValidDriveRoute` | Asserts adding an invalid route throws an exception. | `2, testVehicle.getId(), 1, testStartArea.getId(), -1, testDate, testDate` | Pass |
| `addDriverRoute` & `getAllDriverRoutes` | `getDriverRoutes` | Adds a driver route and asserts the list of all driver routes is not empty and contains the driver route. | `2, testVehicle.getId(), 1, testStartArea.getId(), testEndArea.getId(), testDate, testDate` | Pass |
| `addDriverRoute` & `getUserDriverRoutes` | `getDriverRoutesForOneUser` | Adds a driver route and asserts the list of the users driver routes is not empty and contains the driver route.  | `2, testVehicle.getId(), 1, testStartArea.getId(), testEndArea.getId(), testDate, testDate` & `2` | Pass |
| `addDriverRoute` & `removeDriverRoute` & `getUserDriverRoutes` | `removeDriverRoutes` | Adds and removes a driver route and asserts it is removed successfully and not present in the users driver routes. | `2, testVehicle.getId(), 1, testStartArea.getId(), testEndArea.getId(), testDate, testDate` & `addedRoute.getId()` & `2` | Pass |

### 2.3.2 `DriverRouteResource.java`

| Methods | Test | Description | Json / Query Params | Result |
|:-|:-|:-|:-|:-
| `addDriverRoute()` & `getAllDriverRoutes()` | `getAllDriverRoutes()` | Adds a driver route and asserts the list of all driver routes is not empty and contains it. | `{"vehicleId":testVehicle.getId(), "maxAvailableSeats":4, "startAreaId":testStartArea.getId(), "endAreaId":testEndArea.getId(), "departAt":testDate, "arriveAt":testDate}` | Pass |
| `getAllDriverRoutes()` | `getAllDriverRoutesAsNotAdmin()` | Logs in as non-admin user and asserts trying to get all driver routes throws ``. | | Pass |
| `addDriverRoute()` & `getAllDriverRoutes()` | `addDriverRoute()` | Adds a driver route and asserts it is in the list of all driver routes. | `{"vehicleId":testVehicle.getId(), "maxAvailableSeats":4, "startAreaId":testStartArea.getId(), "endAreaId":testEndArea.getId(), "departAt":testDate, "arriveAt":testDate}` | Pass |
| `addDriverRoute()` | `addDriverRouteAsPassenger()` | Logs in as a passenger and asserts adding a driver route throws `ForbiddenException`. | `{"vehicleId":testVehicle.getId(), "maxAvailableSeats":4, "startAreaId":testStartArea.getId(), "endAreaId":testEndArea.getId(), "departAt":testDate, "arriveAt":testDate}` | Pass |
| `addDriverRoute()` | `addDriverRouteWithInvalidAreas()` | Asserts adding a driver route with an invalid area throws `BadRequestException`. | `{"vehicleId":testVehicle.getId(), "maxAvailableSeats":4, "startAreaId":testStartArea.getId(), "endAreaId":-1, "departAt":testDate, "arriveAt":testDate}` | Pass |
| `addDriverRoute()` & `getUserDriverRoutes()` | `getDriverRoutesForUser()` | Adds a driver route and asserts the list of the users driver routes is not empty and contains it. |  `{"vehicleId":testVehicle.getId(), "maxAvailableSeats":4, "startAreaId":testStartArea.getId(), "endAreaId":testEndArea.getId(), "departAt":testDate, "arriveAt":testDate}` | Pass |
| `addDriverRoute()` & `removeDriverRoute()` | `deleteDriverRouteAsNotAdmin()` | Logs in as a normal user, adds a driver route and asserts trying to remove it throws an `ForbiddenException`. | `{"vehicleId":testVehicle.getId(), "maxAvailableSeats":4, "startAreaId":testStartArea.getId(), "endAreaId":testEndArea.getId(), "departAt":testDate, "arriveAt":testDate}` & `{"routeId":addedDriverRoute.getId()}` | Pass |
| `removeDriverRoute()` | `deleteNonExistingDriverRoute()` | Assserts trying to delete a non existent driver route throws `NotFoundException`. | `{"routeId":addedDriverRoute.getId()}` | Pass |
| `addDriverRoute()` & `removeDriverRoute()` & `getAllDriverRoutes()` | `deleteExistingDriverRoute()` | Adds a driver route and asserts it can successfully be removed and that the list of all driver routes does not contain it. | `{"vehicleId":testVehicle.getId(), "maxAvailableSeats":4, "startAreaId":testStartArea.getId(), "endAreaId":testEndArea.getId(), "departAt":testDate, "arriveAt":testDate}` & `{"routeId":addedDriverRoute.getId()}` | Pass |

### 2.3.3 `PassengerRouteDataAccess.java`

| Methods | Test | Description | Arguments | Result |
|:-|:-|:-|:-|:-
| `addPassengerRoute` & `getUserPassengerRoutes` | `addPassengerRoute` | Adds a passenger route and asserts that the list of all the users passenger routes contains it. | `1, testStartArea.getId(), testEndArea.getId(), testDate, testDate` | Pass |
| `addPassengerRoute` | `addNonValidPassengerRoute` | Asserts trying to add an invalid passenger route throws `DataAccessException`. | `1, testStartArea.getId(), -1, testDate, testDate` | Pass |
| `addPassengerRoute` & `getAllPassengerRoutes` | `getPassengerRoutes` | Adds a passenger route and asserts the list of all passenger routes is not empty and contains it. |  `1, testStartArea.getId(), testEndArea.getId(), testDate, testDate` | Pass |
| `addPassengerRoute` & `getUserPassengerRoutes` | `getPassengerRoutesForOneUser` | Adds a passenger route and asserts the list of all the users passenger routes is not empty and contains it. | `2, testStartArea.getId(), testEndArea.getId(), testDate, testDate` & `2` | Pass |
| `addPassengerRoute` & `removePassengerRoute` & `getUserPassengerRoutes` | `removePassengerRoute` | Adds a passenger route, removes it and asserts it was successfully removed and that the list of all the users passenger routes does not contain it. | `2, testStartArea.getId(), testEndArea.getId(), testDate, testDate` & `addedPassengerRoute.getId()` | Pass |

### 2.3.4 `PassengerRouteResource.java`

| Methods | Test | Description | Json / Query Params | Result |
|:-|:-|:-|:-|:-
| `addPassengerRoute` & `getAllPassengerRoutes` | `getAllPassengerRoutes` | Adds a passenger route and asserts the list of all passenger routes is not empty and contains it. | `{"id":"-1", "userId":"4", "startAreaId":"testStartArea.getId()", "endAreaId":testEndAreaId.getId()", "departAt":"testDate", "arriveAt":"testDate"}` | Pass |
| `getAllPassengerRoutes` | `getAllPassengerRoutesAsNotAdmin` | Logs in as a non-admin user and asserts trying to get all passenger routes throws `ForbiddenException`. | | Pass |
| `addPassengerRoute` & `getAllPassengerRoutes` | `addPassengerRoute` | Adds a passenger route and asserts the list of all passenger routes contain it. |  `{"id":"-1", "userId":"4", "startAreaId":"testStartArea.getId()", "endAreaId":testEndAreaId.getId()", "departAt":"testDate", "arriveAt":"testDate"}` | Pass |
| `addPassengerRoute` | `addPassengerRouteWithInvalidAreas` | Asserts adding a passenger route with an invalid area throws `BadRequestException`. |  `{"id":"-1", "userId":"4", "startAreaId":"testStartArea.getId()", "endAreaId":-1", "departAt":"testDate", "arriveAt":"testDate"}` | Pass |
| `addPassengerRoute` & `getUserPassengerRoutes` | `getPassengerRoutesForUser` | Adds a passenger route and asserts the list of all the users passenger routes is not empty and contain it. | `{"id":"-1", "userId":"4", "startAreaId":"testStartArea.getId()", "endAreaId":testEndAreaId.getId()", "departAt":"testDate", "arriveAt":"testDate"}` | Pass |
| `addPassengerRoute` & `removePassengerRoute` | `deletePassengerRouteAsNotAdmin` | Logs in as a non-admin user and asserts deleting a passenger route throws `ForbiddenException`. | `{"id":"-1", "userId":"4", "startAreaId":"testStartArea.getId()", "endAreaId":testEndAreaId.getId()", "departAt":"testDate", "arriveAt":"testDate"}` & `{"id":"addedPassengerRoute.getId()"}` | Pass |
| `removePassengerRoute` | `deleteNonExistingPassengerRoute` | Asserts removing a non existent passenger route throws `NotFoundException`. | | Pass |
| `addPassengerRoute` & `removePassengerRoute` & `getAllPassengerRoutes` | `deleteExistingPassengerRoute` | Adds a passenger route, removes it and asserts it was successfully removed and that the list of all passenger routes does not contain it. | `{"id":"-1", "userId":"4", "startAreaId":"testStartArea.getId()", "endAreaId":testEndAreaId.getId()", "departAt":"testDate", "arriveAt":"testDate"}` | Pass |


## 2.4 Vehicle
### 2.4.1 `VehicleDataAccess.java`

| Methods | Test | Description | Arguments | Result |
|:-|:-|:-|:-|:-
| `addDriverRoute` & `getAllDriverRoutes` | `getAllDriverRoutes` | Adds a driver route and asserts the list of all driver routes is not empty and contains it. | `{id: -1, userId: -1, vehicleId: testVehicle.getId(), maxAvailableSeats: 4, startAreaId: testStartArea.getId(), endArea: testEndArea.getId(), departAt: testDate, arriveAt: testDate}` | Pass |
| `getAllDriverRoutes` | `getAllDriverRoutesAsNotAdmin` | Logs in as non-admin user and asserts trying to get all driver routes throws an exception. | | Pass |
| `addDriverRoute` & `getAllDriverRoutes` | `addDriverRoute` | Adds a driver route and asserts it is in the list of all driver routes. | `{id: -1, userId: -1, vehicleId: testVehicle.getId(), maxAvailableSeats: 4, startAreaId: testStartArea.getId(), endArea: testEndArea.getId(), departAt: testDate, arriveAt: testDate}` | Pass |
| `addDriverRoute` | `addDriverRouteAsPassenger` | Logs in as a passenger and asserts adding a driver route throws an exception. | `{id: -1, userId: -1, vehicleId: testVehicle.getId(), maxAvailableSeats: 4, startAreaId: testStartArea.getId(), endArea: testEndArea.getId(), departAt: testDate, arriveAt: testDate}` | Pass |
| `addDriverRoute` | `addDriverRouteWithInvalidAreas` | Asserts adding a driver route with an invalid area throws an excetion. | `{id: -1, userId: -1, vehicleId: testVehicle.getId(), maxAvailableSeats: 4, startAreaId: testStartArea.getId(), endArea: -1, departAt: testDate, arriveAt: testDate}` | Pass |
| `addDriverRoute` & `getUserDriverRoutes` | `getDriverRoutesForUser` | Adds a driver route and asserts the list of the users driver routes is not empty and contains it. |  `{id: -1, userId: -1, vehicleId: testVehicle.getId(), maxAvailableSeats: 4, startAreaId: testStartArea.getId(), endArea: testEndArea.getId(), departAt: testDate, arriveAt: testDate}` | Pass |
| `addDriverRoute` & `removeDriverRoute` | `deleteDriverRouteAsNotAdmin` | Logs in as a normal user, adds a driver route and asserts trying to remove it throws an exception. | `{id: -1, userId: -1, vehicleId: testVehicle.getId(), maxAvailableSeats: 4, startAreaId: testStartArea.getId(), endArea: testEndArea.getId(), departAt: testDate, arriveAt: testDate}` & `{routeId: addedDriverRoute.getId()}` | Pass |
| `removeDriverRoute` | `deleteNonExistingDriverRoute` | Assserts trying to delete a non existent driver route throws an exception. | `{routeId: addedDriverRoute.getId()}` | Pass |
| `addDriverRoute` & `removeDriverRoute` & `getAllDriverRoutes` | `deleteExistingDriverRoute` | Adds a driver route and asserts it can successfully be removed and that the list of all driver routes does not contain it. | `{id: -1, userId: -1, vehicleId: testVehicle.getId(), maxAvailableSeats: 4, startAreaId: testStartArea.getId(), endArea: testEndArea.getId(), departAt: testDate, arriveAt: testDate}` & `{routeId: addedDriverRoute.getId()}` | Pass |
| `getUserVehicles` | `getNoVehiclesFromNewUser` | Asserts that the TEST user has no vehicles. | `TEST.getId()` | Pass
| `addVehicle` & `getUserVehicles` | `getUserVehiclesAfterAdd` | Inserts a vehicle for the TEST user and asserts that we successfully retrieve a list of vehicles containing it. | `TEST.getId(), "ABC123", "Volvo", "red", "XC60"` & `TEST.getId()` | Pass
| `addVehicle` & `getVehicle` | `getVehicleByIdAfterAdd` | Inserts a vehicle and asserts that we successfully retrieve it from its ID. | `TEST.getId(), "ABC123", "Volvo", "red", "XC60"` & `newVehicle.getId()` | Pass
| `addVehicle` & `removeVehicle` & `getUserVehicles` | `removeVehicle` | Inserts a vehicle for the TEST user and asserts that it is removed successfully and that it is no longer part of TEST's vehicles. | `TEST.getId(), "ABC123", "Volvo", "red", "XC60"` & `newVehicle.getId()` & `TEST.getId()` | Pass

### 2.4.2 `VehicleResource.java`

| Methods | Test | Description | Json / Query Params | Result |
|:-|:-|:-|:-|:-
| `addVehicle` & `getUserVehicles` | `getUserVehiclesAfterAdd` | Logs in as the ADMIN user, inserts a vehicle, and asserts that it is part of the current user's vehicles. | `{"licensePlate":"ABC123", "brand":"Volvo", "color":"red", "model":"XC60" }` | Pass
| `addVehicle` & `getUserVehiclesById` | `getUserVehiclesByIdAfterAdd` | Logs in as the ADMIN user, inserts a vehicle, and asserts that it is part of ADMIN's vehicles. | `{"licensePlate":"ABC123", "brand":"Volvo", "color":"red", "model":"XC60" }` & `ADMIN.getId()` | Pass
| `addVehicle` & `getVehicle` | `getVehicleAfterAdd` | Inserts a vehicle and asserts that it is the retrieved vehicle of the same ID. | `{"licensePlate":"ABC123", "brand":"Volvo", "color":"red", "model":"XC60" }` & `newVehicle.getId()` | Pass
| `deleteVehicle` | `deleteNonExistingVehicle` | Asserts that we receive a `NotFoundException` if we attempt to delete a vehicle that does not exist. | `-1` | Pass
| `addVehicle` & `deleteVehicle` & `getUserVehicles` | `deleteExistingVehicle` | Logs in as the ADMIN user, inserts a vehicle, and asserts that it is successfully deleted and that it is no longer part of ADMIN's vehicles. | `{"licensePlate":"ABC123", "brand":"Volvo", "color":"red", "model":"XC60" }` & `newVehicle.getId()` | Pass

## 2.5 User
### 2.5.1 `UserDataAccess.java`
No new tests have been added to the base system in `UserDataAccessTest.java`, only modified to fit our changes to the system.

| Methods | Test | Description | Arguments | Result |
|:-|:-|:-|:-|:-
| `addUser` & `getUsers` | `addNewUser` | Inserts a new user and asserts that the retrieved list of all users contains it. | `new Credentials("Generic", "qwerty", Role.PASSENGER), "phone-number", "full-name"` | Pass
| `addUser` | `addDuplicatedUser` | Attempts to inserts two users with the same email and asserts that we receive `DataAccessException`. | `new Credentials("Gandalf", "mellon", Role.PASSENGER), "phone-number", "full-name"` & `new Credentials("Gandalf", "vapenation", Role.PASSENGER), "phone-number", "full-name"` | Pass
| `addUser` | `addShortUser` | Attempts to insert a user with an impossibly short email and asserts that we receive `DataAccessException`. | `new Credentials("Gry", "no", Role.PASSENGER), "phone-number", "full-name"` | Pass
| `getUsers` | `getUsersContainsAdmin` | Asserts that there is at least one user with the ADMIN role. | | Pass
| `deleteUser` | `removeNoUser` | Asserts that deleting a user that does not exist returns `false`. | `-1` | Pass
| `addUser` & `deleteUser` & `getUsers` | `removeUser` | Inserts a user, asserts that they are in the list of users, deletes the user, and finally asserts that they are no longer in the list of users. | `new Credentials("Sven", "a", Role.ADMIN), "phone-number", "full-name"` & `user.getId()` | Pass
| `addUser` & `authenticate` | `authenticateNoUser` | Inserts a new user and asserts that authenticating their credentials produces a valid session associated to them. | `new Credentials("Pelle", "!2", Role.PASSENGER), "phone-number", "full-name"` & `new Credentials("Pelle", "!2", Role.PASSENGER)` | Pass
| `addUser` & `authenticate` | `authenticateNewUserTwice` | Inserts a new user, asserts that authenticating them produces a proper session, and then asserts that authenticating them again produces a separate session. | `new Credentials("Elin", "password", Role.PASSENGER), "phone-number", "full-name"` & `new Credentials("Elin", "password", Role.PASSENGER)` | Pass
| `removeSession` | `removeNoSession` | Attempts to remove a session with a random UUID and asserts that `false` is returned. | `UUID.randomUUID()` | Pass
| `addUser` & `authenticate` & `removeSession` | `removeSession` | Inserts a user, authenticates them, and then asserts that removing the session twice first returns `true` and then `false`. | `new Credentials("MormorElsa", "kanelbulle", Role.PASSENGER), "phone-number", "full-name"` & `new Credentials("MormorElsa", "kanelbulle", Role.NONE)` & `session.getSessionId()` | Pass
| `addUser` & `authenticate` | `failedAuthenticate` | Inserts a user and asserts that authenticating as them with the wrong password yields `DataAccessException` | `new Credentials("steffe", "kittylover1996!", Role.PASSENGER), "phone-number", "full-name"` & `new Credentials("steffe", "cantrememberwhatitwas! nooo!", Role.NONE)` | Pass
| `addUser` & `authenticate` & `getSession` | `checkSession` | Inserts a user, authenticates as them, and asserts that retrieving their session by ID yields a valid session associated to them. | `new Credentials("uffe", "genius programmer", Role.ADMIN), "phone-number", "full-name"` & `new Credentials("uffe", "genius programmer", Role.ADMIN)` & `session.getSessionId()` | Pass
| `addUser` & `authenticate` & `getSession` & `removeSession` | `checkRemovedSession` | Inserts a user, authenticates as them, asserts that retrieving the session works, removes the session, and finally asserts that retrieving the session now throws `DataAccessException`. | `new Credentials("lisa", "password", Role.PASSENGER), "phone-number", "full-name"` & `new Credentials("lisa", "password", Role.PASSENGER)` & `session.getSessionId()` & `session.getSessionId()` | Pass
| `getUser` | `getUser` | Asserts that retrieving a user with an ID returns a user with said ID. | `1` | Pass
| `getUser` | `getMissingUser` | Asserts that retrieving a missing user throws `DataAccessException`. | `-1` | Pass
| `updateUser` | `updateMissingUser` | Asserts that updating a missing user throws `DataAccessException`. | `10, new Credentials("admin", "password", Role.ADMIN), "", ""` | Pass
| `updateUser` | `updateUser` | Asserts that updating the TEST user's email, password, and role returns a user with those attributes. | `2, new Credentials("test2", "newpass", Role.PASSENGER), "", ""` | Pass
| `authenticate` & `updateUser` & `authenticate` | `updateWithoutPassword` | Authenticates as the TEST user, updates the email to `"test2"` and password to `null`, then authenticates as before but this time with the new email, and finally prints the two returned sessions. | `new Credentials("Test", "password", Role.PASSENGER)` & `2, new Credentials("test2", null, Role.PASSENGER), "", ""` & `new Credentials("test2", "password", Role.PASSENGER)` | Pass

### 2.5.2 `UserResource.java`
No new tests have been added to the base system, only modified to fit our changes. Does not include all test in `UserResourceTest.java`.

| Methods | Test | Description | Json / Query Params | Result |
|:-|:-|:-|:-|:-
| `currentUser` | `notAuthenticatedCurrentUser` | Asserts non authenticated user has the role `NONE`. | | Pass
| `getUsers` | `notAuthenticatedGetAllUsers` | Asserts that a non authenticated user trying to get all users throws `ForbiddenException`. | | Pass
| `login` & `currentUser` | `loginCookies` | Logs in and asserts the cookie contains the correrct domain, user and path aswell as retrieving the user with no cookie returns a user with role `NONE` and retrieving with a cookie returns a user with correct role. | `{crdentials: new Credentials("Test", "password". Role.DRIVER)}` | Pass 
| `registerUser` | `registerUser` | Registers a new user and asserts all their account information is corrected. | `{register: new UserWrapper("pelle", "passphrase", "PASSENGER", "full-name", "112")}` | Pass 
| `registerUser` | `registerAdminRole` | Asserts trying to register with the role `ADMIN` throws `BadRequestException`. | `{register: new UserWrapper("pelle", "passphrase", "PASSENGER", "full-name", "112")}` | Pass
| `registerUser` | `registerNoneRole` | Asserts trying to register with the role `NONE` throws `BadRequestException`. | `{register: new UserWrapper("pelle", "passphrase", "PASSENGER", "full-name", "112")}` | Pass

## 2.6 Frontend Jasmine
### 2.6.1 `add-vehicle.js`

| Methods | Test | Description | Arguments | Result |
|:-|:-|:-|:-|:-
| `addVehicle` | should call `addVehicle` on form submit | Fills out the fields for adding a new vehicle and asserts `addVehicle` is called when the submit-button is pressed. | | Pass
| `rest.addVehicle` | should call `rest.addVehicle` on form submit | Fills out the fields for adding a new vehicle and asserts `addVehicle` is called when the submit-button is pressed. | `'Tesla', 'S', 'ABC 123', 'blue'` | Pass

### 2.6.2 `register.js`

| Methods | Test | Description | Arguments | Result |
|:-|:-|:-|:-|:-
| `registerUser` | should call `registerUser` on form submit and change | Fills out the fields for registering a new account and asserts `registerUser` is called when the register-button is pressed. | | Fail
| `changeLocation` | should change location to login on registration submit | Fills out the fields for registering a new account and asserts the page is changed to the login page when the register-button is pressed. | | Fail
| `register` | should call `register` in rest on form submit | Fills out the fields for registering a new account and asserts `register` is called when the register-button is pressed. | `'Fornamn Efternamn', '0700000000', 'email@domain.com', 'password1', 'Both'` | Fail
| `changeLocation` & `rest.login` | should be able to login a new user | Asserts `rest.login` and `changeLoction` is called when the login-button is pressed. | `'/' & 'email@domain.com', 'password1'` | Fail 


