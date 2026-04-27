Read `framework/AGENTS.md` and `framework/CODING_STANDARDS.md`  
Always adhere to the instructions outlined in above files, unless stated otherwise.

Study the documentation found in `framework/docs`

Before anything else, the database connection must be set up.  
Verify env vars are available, and abort further implementation.  
In the event any of the below variables are not available.  

The following env vars will be available.  
use `sed` to replace into `framework/storage/config/global.jsonc`  
- `DB_HOST` Hostname of the database
- `DB_USER` Username for the database
- `DB_PASS` Password for the database user
- `DB_NAME` Name of the database to use

The codebase in `framework/` is intended to be integrated with using API's
You must therefore implement the following:

**API controller**  
A global API controller that re-routes every `/api/*` request to a namespaced api-controller.  
Leverage on-top-off the built-in request/response handling.  
Maintain support client preffered response type via `Accept:` header and the `RespondWith` attribute for developer to override.  

fx.  
- /api/product 		  -> \Api\ProductController::index()  
- /api/product/delete -> \Api\ProductController::delete()  
- /api/product/create -> \Api\ProductController::create()  

Implement a simple `/api/ping` endpoint for integrators to use when validating API keys.  
Therefore authentication should be required.  

Build documentation and sdk when endpoint is implemented.  

**API authentication**  
Implement API key authentication using header `X-Api-Key`.  
Assume API keys will be UUIDv4's  
Authentication must happen in the global `ApiController`  

Create the following DB table:
- Table: `apiKey`
	- Col: `apiKey` (primary, UUIDv4)
	- Col: `comment` (Human readable comment)
	- Col: `lastUsed` (datetime)

**API logging**
Create the following DB table:
- Table: `apiLog`
	- Col: `logId` (primary, UUIDv4)
	- Col: `requestMethod`
	- Col: `requestUri` (add index)
	- Col: `requestBody`
	- Col: `requestHeaders`
	- Col: `responseCode` (Response HTTP code)
	- Col: `responseBody`
	- Col: `createdAt` (datetime, add index)

Only update `lastUsed` on successful authentication.  
Response headers must always contain `X-Log-Id` header, containing the UUID from the logged entry.

Insert log-entry early in request-phase.  
Update with response later.  

**API testing**  
Scaffold a new testsuite in `framework/tests/api`

Currently provided `test` command should always run all testsuites configured.
Add a new `test:api` command to `framework/composer.json` that runs only api tests.  

Generate a UUIDv4 api key and insert in the `apiKey` table.

**API classes**  
Classes in the `\Api` namespace not directly related to endpoints, must be in a subfolder.  
Avoid the use of static methods in non-endpoint classes.  

Following classes are required:
- \Api\Support\Key - For API key validation, extends \Entity, use inherited exists)
- \Api\Support\Validator - For request method assertions (and other needed validation)
- - Implements: `assertMethod($expected)`

**API documentation**  
Create a `/api-documentation` controller.  
In a corrosponding view file `stoplightio/elements` must be implement using a web component.
Refering to the `.json` file built by `composer run api:doc` command below.  

Add the following composer commands to `framework/composer.json`.  
Rationale:  Manual implementation introduces unjustifiable complexity.  

*api:doc*
Generates an OpenAPI compatible `.json` file, read from source files in \Api namespace.
Uses `zircote/swagger-php` for generating `openapi.json` spec file.
Clone `swagger-php` into the `framework/` folder
The generated `openapi.json` file must be written to `framework/src/openapi.json`.

*api:sdk*
Generates a PHP sdk.  
Uses `OpenAPITools/openapi-generator`  
Clone `openapi-generator` into the `framework/` folder.  
The generated API client must be written to `framework/api-client`  

**Ask questions**
Ask questions during planning, if any discrepencies are found
between those instructions and what the framework provides.

Ask and suggest alternatives in the event any of the above proves tricky to complete.  
fx. multiple commands failing in a row.  

**Further instructions**  
Add the following agent instructions to `framework/AGENTS.md`.  
Below instructions also apply to this implementation sprint.  

```
## API endpoints
Annotate every new endpoint using OpenAPI PHP attributes.  
Following points is required to have documented for each endpoint.  
- Path  
- Request method  
- Parameters/Properties  
- Response  

## API request/response contract
All API responses must be JSON and follow this structure:
- Errors: `{"errors": [ ... ]}`
- Success: `{"success": true, "result": { ... }}`
- `result` must always be an object containing relevant properties

## API documentation
Creating or updating endpoints, must always result in docs and the sdk being updated.  
Use composer commands `composer run api:doc` and `composer run api:sdk` accordingly.  

## Unit testing
After each implementation run, unit tests must be run, and should always pass.  
For complex errors, ask developer for preferred directions.  

If only files in the `\Api` namespace is altered, running `api:test` is fair game.  
Otherwise always use the `test` command that will cover all test suites.  

New endpoints requires new tests in the `framework/tests/api/` directory.  
```