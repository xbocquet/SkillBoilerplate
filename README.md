
# Boilerplate for remote expertise

## Table of Contents

* [JSON Interface Reference](#json-interface-reference)
* [API Reference](#api-reference)
* [Intents](#intents)
* [NLU](#nlu)
* [Languages](#Languages)
* [Logging](#logging)
* [Tests](#tests)
* [Code Style](#code-style)
* [Tools](#tools)

## JSON Interface Reference

### [Manifest](./manifest.json)

This section defines the expertise configuration which is used by the core to access and control the expertise.

``` json
{
    "name": "string",
    "description": "string",
    "private": false,
    "author": "string",
    "version": "string",
    "license": "string",
    "callword": "string",
    "lifetime": "string",
    "threshold": 0,
    "languages": [
      "string", "string"
    ],
    "tags": [
      "string"
    ],
    "nlu": [
      "string"
    ],
    "services": [
      {
        "name": "string",
        "description": "string",
        "url": "string",
        "attributes": [
          {
            "name": "string",
            "type": "string",
            "required": false
          },
          {
            "name": "string",
            "type": "string",
            "required": true
          }
        ]
      }
    ]
}
```

#### Parameters Description

Parameter | Description | Type | Required
--- | --- | --- | ---
name | The expertise name | string | Yes
description | Description of the expertise | string | No
private | false for common expertise | boolean | no
author | Author name | string | No
version | Expertise version | string | Yes
license | | string | No
languages | Supported languages by the expertise | string array | no
tags | | string array | No
nlu | Supported NLU engines | string array | No
services | | | No

#### Expertise Version

If an expertise is going to be published, it should start at 1.0.0, though it is not mandatory.

After this, changes should be handled as follows:

- (PATCH) Bug fixes and other minor changes: Patch release, increment the last number, e.g. 1.0.1
- (MINOR) New features which don't break existing features: Minor release, increment the middle number, e.g. 1.1.0
- (MAJOR) Changes which break backwards compatibility: Major release, increment the first number, e.g. 2.0.0

See [Semantic Versioning](http://semver.org/) for more information.

### Request

This defines the request interface that is sent to the expertise action handler function

``` json
{
  "id": "string",
  "version": "string",
  "language": "string",
  "text": "string",
  "retext": "string",
  "attributes": {
  },
  "context": {
    "user" : {
      "id": "string"
    },
    "session" : {
      "id": "string",
      "new": true,
      "attributes" : {
      }
    },
    "application" : {
      "id": "string",
      "attributes": {
      }
    }
  }
}
```

#### Parameters Description

Parameter | Description | Type | Required
--- | --- | --- | ---
id | request id | string | yes
version | Request version | string | yes
language | User text query language | string | no
text| User query as text | string | yes
retext | User query after text reformatting | string | yes
attributes | The extracted intent and entities from user request | object | yes
context: Request context data | object | yes

##### Request Context Types

Context Type | Description
--- | ---
user |
application | Application specific context
session |

###### User Context Object

Parameter | Description | Type | Required
--- | --- | --- | ---
id | User unique id | string | yes

###### Application Context Object

Parameter | Description | Type | Required
--- | --- | --- | ---
id | Application unique id | string | yes
attributes | | object | yes

###### Session Context Object

Parameter | Description | Type | Required
--- | --- | --- | ---
new | | boolean | yes
attributes | | object | yes


### Response

This defines the format of the response that that the expertise should return to the interact core.

``` json
{
  "version": "string",
  "reject": "boolean",
  "error": "number",
  "shouldEndSession": "boolean",
  "speech": {
    "text": "string",
    "display": "string",
    "expressiveness": "string"
  },
  "card": {
    "type": "string",
    "title": "string",
    "subtitle": "string",
    "content": {
    }
  },
  "context": {
    "user": {
    },
    "session": {
    },
    "application": {
    }
  },
  "metrics": {
    "expertise": "string"
  }
}
```

#### Parameters Description

Parameter | Description | Type | Required
--- | --- | --- | ---
version | Response version | string | yes
reject | | boolean | yes
shouldEndSession | | boolean | yes
context | | object | yes

#### Speech Object

Parameter | Description | Type | Required
--- | --- | --- | ---
text | | string | yes
expressiveness | | string | yes

#### Card Object

Parameter | Description | Type | Required
--- | --- | --- | ---
type | | string | yes
title | | string | yes
subtitle | | string | yes
content | | object | no

#### Response Context Types

Context | Description
--- | ---
user |
session |

## API Reference

Method | API | Description
--- | --- | ---
GET | /v1/api/manifest | Returns the expertise manifest as a JSON object.
GET | /v1/api/nlu | Returns NLU metadata which is used by the interact core NLU engines.
GET | /v1/api/intents | Returns expertise intents descriptors.
GET | /v1/api/healthCheck | Check the status (health) of the expertise.
POST | /v1/api/converse | Get a response from user input or event.

## NLU

The Natural Language Understanding includes data required by the Interact core to extract intents and entities.

### Intents Descriptors

The intents file is optional metadata file which includes expertise intent descriptors. The information in this file is applied by the core to extracted intent (after NLU extraction phase).
The expertise developer can use the intents descriptors to increase the accuracy of the expertise and to control intent behavior.

``` json
{
  "<intent name>" :
  {
    "visibility" : "string",
    "entities" : [
      {
        "name" : "string",
        "required" : "boolean"
      }
    ]
  }
}
```

Parameter | Description | Type | Required
--- | --- | --- | ---
visibility | Can be use to hide or show intent to core | string | No

### Watson Conversation Service (WCS)

``` json
{
  "workspace": {
    "en-US": {
      "name:": "string",
      "workspaceId": "string"
    },
    "de-DE": {
      "name": "string",
      "workspaceId": "string"
    }
  }
}
```

### Regular expressions

### API.AI

## Languages

TBD

## Logging

TBD

## Running

### Locally

To run expertise locally you can use the following command

```
npm start
```

**Note!** Make sure the port is not used by any other expertise / server as the expertise will not start.

### Docker

#### Prerequisites

* [Install Docker](https://docs.docker.com/engine/installation/)

#### Locally

- Build the image
```
docker build -t expertise-boileplate:local .
```

- Create and run a container
```
docker run --name fun -p 3000:3000 -e PORT=3000 -it expertise-boileplate:local
```

#### Bluemix

For more information how to build and run containers in Bluemix you can refer to [Adding Docker images to your organization's private Bluemix images registry](https://console.ng.bluemix.net/docs/containers/container_images_adding_ov.html#container_images_adding_ov)

- Build your image and push it to your private Bluemix registry.
```
bx ic build -t registry.ng.bluemix.net/<namespace>/expertise-boileplate:v1.0.0 .
```
**Tip:** Run `bx ic namespace get` to retrieve your namespace information.

- Create a container from your image.
```
bx ic create --name expertise-boileplate -p 3001:3001 -e PORT=3001 registry.ng.bluemix.net/<namespace>/expertise-boileplate:v1.0.0
```
**Note:** Select port that will not conflict with other expertise

- If can be published as public expertise, bind a public IP address to your container.
```
bx ic bind <ip address> <container ID>
```
**Tip:** You can see your public IP addresses by running `bx ic ips`. To request a new one, run `bx ic ip request`.

## How To Manually Test Expertise

The expertise (locally or hosted) can be tested directly with the expertise converse REST API. Use the following json block in the expertise swagger page.

```
{
  "id": "001",
  "text": "hello world",
  "retext": "hello world",
  "version": "1.0",
  "language": "en-US",
  "attributes": {
    "intent": "hello-world"
  },
  "context": {
    "user": {
      "id": "user-001"
    },
    "application": {
      "id": "app-001",
      "attributes": {
      }
    },
    "session": {
      "new": true,
      "attributes": {
      },
      "version": "1.0"
    }
  }
}
```

## Tests

Running all the tests:
```
$ npm test
```

## Maintainers

Eyal Cohen - eyalco@il.ibm.com
