# shs-click-sys
*Source Code repo:* [*shs-click-sys*](https://github.com/searshomeservices/shs-click-sys)


## Builds

*Note: the build status is only available on the Sears home services network.*


| Environment   | Status        | 
| :-------------: |:-------------:| 
| Development   | *not implemented yet* | 


## Description
Provided functionality for the click and attribute engine apis.

## Features
Sends a request to the click and attribute engine apis on behalf of the requester and formats the response according to the common data model.

## Healthcheck

- Healthcheck endpoint is available at the path below.  Only the timestamp feature is implemented.
```/```

------

# Development, Testing, & Operations

## Configuration

### API Properties
- `api.id="123456"`:  Set to the api id once deployed and added to API manager.

## Flow descriptions

- error-handling: flows to handle API errors.
- global.xml: container for all configurations for the API.

## Dependencies

- Click API
- Attribute Engine API
- Auth Token API

## Testing

- MUnit tests available at `src/test/munit`
- Postman Test API collection is available at `src/test/resources/test-postman-collection.json`
