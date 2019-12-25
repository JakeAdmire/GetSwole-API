###### Top

<br />
<p align="center">
  <h1 align="center">GetSwole API</h1>
  <p align="center">
    Organize your fitness!
    <br />
    <br />
    <b><a href="https://em-ja-palette-picker.herokuapp.com/">View Demo</a></b>
  </p>
</p>
<div align="center">

[![Travis][travis-shield]][travis-url] 
[![Heroku][heroku-shield]][heroku-url] 

[![LinkedIn][linkedin-shield]][linkedin-url] 
[![Gmail][gmail-shield]][gmail-url] 
[![GitHub][github-shield]][github-url] 
</div>

## Table of Contents

- [Getting Started](#Getting-Started)
  - [Prerequisites](#Prerequisites)
  - [Installation]()
  - [Setting up the database]()
- [Usage]()
  - [Directory]()
  - [User]()
  - [Exercise(s)]()
  - [Routine(s)]()

## Getting Started

> If you would like to use the API in production mode, you can skip to the [usage](#usage) section.

### Prerequisites

* npm
```sh
npm install npm@latest -g
```

---

### Installation

1. Clone the repo
```sh
git clone https://github.com/JakeAdmire/GetSwole-API
```
2. Install Gem packages
```sh
cd GetSwole-API && bundle install
```
---

### Setting Up The Database

1. Create and migrate:
```sh
rake db:{create,migrate}
```
2. Import the exercises from the .csv file in the /lib directory:
```sh
rake import:exercises
```
3. Seed the other db items in the seed file:
```sh
rake db:seed
```
4. Run the code in development mode:
```sh
rails s
```
5. Open your browser and visit http://localhost:3000

## Usage

> If you would like to use the development environment and see the code, read through the [Getting Started](#Getting-Started) section.

*All endpoints adhere to the Fast JSON API standards*

[Base URL](https://warm-cove-89223.herokuapp.com/api/v1) - `https://warm-cove-89223.herokuapp.com/api/v1`

### Usage Directory

- [User]()
  - [[POST] Create a new User](#post-a-user)
  - [[POST] Authenticate an existing User]()
- [Exercise(s)]()
  - [[GET] Get all Exercises]()
  - [[GET] Get a specific Exercise]()
  - [[POST] Add an existing Exercise to an existing Routine]()
  - [[DELETE] Remove an existing Exercise from an existing Routine]()
 - [Routine(s)]()
   - [[GET] Get all Routines]() 
   - [[GET] Get a specific Routine]()
   - [[GET] Get all scheduled Routines for a set date]()
   - [[POST] Create a new Routine]()
   - [[POST] Schedule a Routine]()
   - [[PUT] Update an existing Routine]()
   - [[DELETE] Delete an existing Routine]()
   - [[DELETE] Unschedule a Routine]()

## User

### [POST] Create a new `User`

**Request:**
`POST /users`

**Body:**
```json
{
	name: <STRING>,
	email: <STRING>,
	password: <STRING>,
	password_confirmation: <STRING>
}
```
Ex:
```json
{
	name: "Jim",
	email: 'email@email.com',
	password: '1',
	password_confirmation: '1'
}
```
> NOTE: The `password` **must** match the `password_confirmation`.

**Response:**
```json
{
	api_key: <STRING>
}
```

---

### [POST] Authenticate an existing `User`

**Request:**
`POST /login`

**Body:**
```json
{
	email: <STRING>,
	password: <STRING>
}
```
Ex:
```json
{
  email: 'email@email.com',
  password: '1'
}
```
> NOTE: The `email` & `password` combo **must** match the existing login information.

**Response:**
```json
{
	name: <STRING>,
	api_key: <STRING>
}
```

## Exercise(s)

### [GET] Get all `Exercises`

**Request:**
`GET /exercises`
> No request body is required.

**Response:**
```json
[
	{
		name: <STRING>,
		sets?: 
		[
			{
				reps?: <ARRAY>,
				duration?: <STRING>
			},
			...
		],
		duration?: <STRING>
	},
	...
	
]
```
---

### [GET] Get a specific `Exercise`

**Request:**
`GET /exercises/:id`
> No request body is required.

**Response:**
```json
{
	name: <STRING>,
	sets?: 
	[
		{
			reps?: <ARRAY>,
			duration?: <STRING>
		},
		...
	],
	duration?: <STRING>
}
```
---

### [POST] Add an existing `Exercise` to an existing `Routine`

**Request:**
`POST /exercise_routines`

**Body:**
```json
{
  exercise_id: <INTEGER>,
  routine_id: <INTEGER>
}
```
Ex:
```json
{
  exercise_id: 3,
  routine_id: 5
}
```
---

### [DELETE] Remove an existing `Exercise` from an existing `Routine`

**Request:**
`DELETE /exercise_routines/:id`

## Routine(s)

### [GET] Get all `Routines`

**Request:**
`GET /routines`
> No request body is required.

**Response:**
```json
[ 
	{
		name: <STRING>,
		exercises: <ARRAY>
	},
	...
]
```

---

### [GET] Get a specific `Routine`

**Request:**
`GET /routines/:id`
> No request body is required.

**Response:**
```json
{
	name: <STRING>,
	exercises: <ARRAY>
}
```
---

### [GET] Get all scheduled `Routines` for a set date

**Request:**
`GET /my_routines?date=<DATE>&user_id=<ID>&api_key=<KEY>`

**Response:**
```json
[
	{
		name: <STRING>,
		exercises: <ARRAY>
	},
	...
]
```
---

### [POST] Create a new `Routine`

**Request:**
`POST /routines?user_id=<USER_ID>&api_key=<USER_API_KEY>`
> No request body is required.

**Body:**
```json
{
  name: <STRING>,
  exercises?: <ARRAY>
}
```
Ex:
```json
{
  name: "Leg Day",
  exercises?: [1, 3, 56, 345]
}
```
---

### [POST] Schedule a `Routine`

**Request:**
`POST /my_routines`

**Body:**
```json
{
  date: <STRING>,
  routine_id: <INTEGER>,
  user_id: <INTEGER>,
  api_key: <STRING>
}
```

Ex:
```json
{
  date: "2019-05-22",
  routine_id: 2,
  user_id: 1,
  api_key: '123456789'
}
```
---

### [PUT] Update an existing `Routine`

**Request:**
`PUT /routines/:id`

**Body:**
```json
{
  name: <STRING>
}
```

Ex:
```json
{
  name: "Leg Day 2.0"
}
```
---

### [DELETE] Delete an existing `Routine`

**Request:**
`DELETE /routines/:id`

---

### [DELETE] Unschedule a `Routine`

**Request:**
`DELETE /my_routines?routine_id=<ID>&user_id=<ID>&date=<DATE>&api_key=<KEY>`

## Running Tests

**GetSwole-API** uses [RSPEC](https://rspec.info/) as a testing suite. 

To run the test suite, after completing the steps from [Getting Started](#Getting-Started) , simply run:
```sh
rspec
```

To view the coverage report (after running the test suite):
```sh
open coverage/index.html
```
> This will open a file in your browser that will show details about test coverage.
---

This project was assigned by David Whitaker and Will Mitchell

_@ Turing School of Software & Design, Denver, CO._

---

**[BACK TO TOP](#top)**

<!-- URL References  -->
[linkedin-shield]: https://img.shields.io/badge/-LinkedIn-0077b5.svg?style=for-the-badge&logo=linkedin
[linkedin-url]: https://linkedin.com/in/jakeadmire

[gmail-shield]: https://img.shields.io/badge/-Email-red.svg?style=for-the-badge&logo=gmail&logoColor=white
[gmail-url]: mailto:jakeadmire1@gmail.com

[github-shield]: https://img.shields.io/badge/dynamic/json?label=Follow&query=length&url=https://api.github.com/users/jakeadmire/followers&style=for-the-badge&logo=github
[github-url]: https://github.com/JakeAdmire/

[travis-shield]: https://img.shields.io/travis/criteriamor/Palette-Picker-API?label=travis-ci&logo=travis&style=for-the-badge
[travis-url]: https://travis-ci.org/JakeAdmire/GetSwole

[heroku-shield]: https://img.shields.io/badge/expo-deployed-lightblue?style=for-the-badge&logo=expo
[heroku-url]: https://expo.io/@jakeadmire/getSwole
