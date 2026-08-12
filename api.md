# Spacedock API Docs

Spacedock has a simple HTTP API that you can use to do various interesting
things. Feel free to help make it better by submitting pull requests that update
[api.py](https://github.com/SirCmpwn/KerbalStuff/blob/master/KerbalStuff/blueprints/api.py).

## Basics

Submit all POSTS with the request body encoded as
[multipart/form-data](https://www.ietf.org/rfc/rfc2388.txt). Your HTTP library
of choice probably handles that for you. All responses are JSON.

Please set your user agent to something that describes who you are and how to
contact the person operating the service.

#### Errors

All requests that might fail include an `error` property in the response, which
is a boolean that will be true if the request failed. If the request failed, a
`reason` property will also be included that explains why it failed.

<details><summary><i>Example Error</i></summary>

  ```json
  {
    "error": true,
    "reason": "Username or password is incorrect"
  }
  ```
</details>

<br />


## Authentication

Some endpoints require authentication. To authenticate, use the login endpoint
and you will be given a cookie, which you should include in all subsequent
requests.

#### POST /api/login

Logs into Spacedock.

*Curl*

```sh
curl -F username=SirCmpwn -F password=example -c ./cookies "https://spacedock.info/api/login"
```

*Parameters (Form Data)*

* `username`
* `password`

<details><summary><i>Example Response</i></summary>

```json
{
  "error": false 
}
```
</details>

<details><summary><i>Errors</i></summary>

| Code | Reason | 
| --- | --- |
| 401 | Missing username or password |
| 401 | Username or password is incorrect |
| 403 | User is not confirmed |
</details>

<br />

## Browse

You can browse the site without authentication.

### GET /api/browse

Gets mods sorted by selected conditions

*Curl*

```sh
curl "https://spacedock.info/api/browse"
```

*Parameters (Search Params)*

* `game_id`: Only return mods for this game, by internal database id [*optional*]
* `game_version`: Only return mods for this game version, by friendly string [*optional*]
* `game_version_id`: Only return mods for this game version, by internal database id [*optional*]
* `page`: Which page of results to retrieve (1 indexed) [*optional*]
* `orderby`: Which property of mod use for ordering. Valid values: name, updated, created. Default: created. [*optional*]
* `order`: Which ordering direction to use. Valid values: asc, desc. Default: asc. [*optional*]
* `count`: Which count of mods to show per page. Valid values: 1-500. Default 30. [*optional*]

If `game_version_id` is present, `game_id` and `game_version` will be ignored.

<details><summary><i>Example Response</i></summary>

```json
{
  "result": [
    {
      "downloads": 27885,
      "name": "Ferram Aerospace Research",
      "followers": 177,
      "author": "ferram4",
      "default_version_id": 295,
      "versions": [
        {
          "changelog": "...",
          "game_version": "0.24.2",
          "download_path": "/mod/52/Ferram%20Aerospace%20Research/download/v0.14.1.1",
          "id": 151,
          "friendly_version": "v0.14.1.1"
        }
      ],
      "id": 52,
      "background": "...",
      "bg_offset_y": 1234,
      "short_description": "..."
    },
    ...continued...
  ],
  "count": 30,
  "pages": 100,
  "page": 1
}
```
</details>

<br />

### GET /api/browse/new

Gets the newest mods on the site.

*Curl*
```sh
curl "https://spacedock.info/api/browse/new"
```

*Parameters (Search Params)*

* `page`: Which page of results to retrieve (1 indexed) [*optional*]
* `game_id`: Only return mods for this game, by internal database id [*optional*]
* `game_version`: Only return mods for this game version, by friendly string [*optional*]
* `game_version_id`: Only return mods for this game version, by internal database id [*optional*]

If `game_version_id` is present, `game_id` and `game_version` will be ignored.

<details><summary><i>Example Response</i></summary>
```json
    [
      {
        "downloads": 27885,
        "name": "Ferram Aerospace Research",
        "followers": 177,
        "author": "ferram4",
        "default_version_id": 295,
        "versions": [
          {
            "changelog": "...",
            "game_version": "0.24.2",
            "download_path": "/mod/52/Ferram%20Aerospace%20Research/download/v0.14.1.1",
            "id": 151,
            "friendly_version": "v0.14.1.1"
          }
        ],
        "id": 52,
        "background": "...",
        "bg_offset_y": 1234,
        "short_description": "..."
      },
      ...continued...
    ]
```
</details>

<br />

### GET /api/browse/featured

Gets the latest featured mods on the site.

*Curl*
```sh
curl "https://spacedock.info/api/browse/featured"
```

*Parameters (Search Params)*

* `page`: Which page of results to retrieve (1 indexed) [*optional*]

<details><summary><i>Example Response</i></summary>

```json
[
  {
    "downloads": 27885,
    "name": "Ferram Aerospace Research",
    "followers": 177,
    "author": "ferram4",
    "default_version_id": 295,
    "versions": [
      {
        "changelog": "...",
        "game_version": "0.24.2",
        "download_path": "/mod/52/Ferram%20Aerospace%20Research/download/v0.14.1.1",
        "id": 151,
        "friendly_version": "v0.14.1.1"
      }
    ],
    "id": 52,
    "background": "...",
    "bg_offset_y": 1234,
    "short_description": "..."
  },
  ...continued...
]
```
</details>

<br />

### GET /api/browse/top

Gets the most popular mods on the site.

*Curl*
```sh
curl "https://spacedock.info/api/browse/top"
```
*Parameters (Search Params)*

* `page`: Which page of results to retrieve (1 indexed) [*optional*]

<details><summary><i>Example Response</i></summary>

```json
[
  {
    "downloads": 27885,
    "name": "Ferram Aerospace Research",
    "followers": 177,
    "author": "ferram4",
    "default_version_id": 295,
    "versions": [
      {
        "changelog": "...",
        "game_version": "0.24.2",
        "download_path": "/mod/52/Ferram%20Aerospace%20Research/download/v0.14.1.1",
        "id": 151,
        "friendly_version": "v0.14.1.1"
      }
    ],
    "id": 52,
    "background": "...",
    "bg_offset_y": 1234,
    "short_description": "..."
  },
  ...continued...
]
```
</details>

<br />

### GET /api/typeahead/mod

Endpoint used to provide typeahead functionality when searching a mod

*Curl*

```sh
curl "https://spacedock.info/api/typeahead/mod?game_id=22409&query=Compe"
```

*Parameters (Search Params)*

* `game_id`: Only return mods for this game, by internal database id
* `query`: Search terms

<details><summary><i>Example Response</i></summary>

```json
[
  {
    "name": "Compendium",
    "id": 4074,
    "game": "Kitten Space Agency",
    "game_id": 22409,
    "short_description": "Celestial body informational window, orbital line toggle management!",
    "downloads": 892,
    "followers": 0,
    "author": "nanonestor",
    "default_version_id": 24674,
    "shared_authors": [],
    "background": "https://spacedock.info/content/nanonestor_179255/Compendium/Compendium-1765528302.png",
    "bg_offset_y": -817,
    "license": "MIT",
    "website": "https://github.com/meow-sci/Compendium",
    "donations": "",
    "source_code": "https://github.com/meow-sci/Compendium",
    "url": "/mod/4074/Compendium",
    "versions": [
      {
        "friendly_version": "0.9.10",
        "game_version": "2026.8.5.5168",
        "id": 24674,
        "created": "2026-08-07T15:46:11.004784+00:00",
        "download_path": "/mod/4074/Compendium/download/0.9.10",
        "changelog": "- Updated to accommodate changed KSA code",
        "downloads": 26
      },
      ...continue...
    ]
  }
]
```
</details>

<br />


## Search

You can search the site without authentication.

### GET /api/search/mod

Searches the site for mods.

*Curl*
```sh
curl "https://spacedock.info/api/search/mod?query=FAR"
```
*Parameters (Search Params)*

* `query`: Search terms
* `page`: Which page of results to retrieve (1 indexed) [*optional*]

<details><summary><i>Example Response</i></summary>

```json
[
  {
    "downloads": 27885,
    "name": "Ferram Aerospace Research",
    "followers": 177,
    "author": "ferram4",
    "default_version_id": 295,
    "versions": [
      {
        "changelog": "...",
        "game_version": "0.24.2",
        "download_path": "/mod/52/Ferram%20Aerospace%20Research/download/v0.14.1.1",
        "id": 151,
        "friendly_version": "v0.14.1.1"
      }
    ],
    "id": 52,
    "background": "...",
    "bg_offset_y": 1234,
    "short_description": "..."
  }
]
```
</details>
<br />

### GET /api/search/user

Searches the site for public users.

*Curl*
```sh
curl "https://spacedock.info/api/search/user?query=sircmpwn"
```

*Parameters (Search Params)*

* `query`: Search terms
* `page`: Which page of results to retrieve (1 indexed) [*optional*]

<details><summary><i>Example Response</i></summary>

```json
[
  {
    "username": "SirCmpwn",
    "twitterUsername": "sircmpwn",
    "mods": [],
    "redditUsername": "",
    "ircNick": "sircmpwn",
    "description": "Hi, I made this website.",
    "forumUsername": "SirCmpwn"
  }
]
```
</details>
<br />

## Users

You can query the API for information on individual public users.

### GET /api/user/\<username>

Returns information about a specific user.

*Curl*
```sh
curl "https://spacedock.info/api/user/Xaiier"
```

<details><summary><i>Example Response</i></summary>

```json
    {
      "username": "Xaiier",
      "twitterUsername": "",
      "mods": [
        {
          "downloads": 332,
          "name": "Time Control",
          "followers": 19,
          "author": "Xaiier",
          "default_version_id": 371,
          "id": 21,
          "short_description": "..."
        }
      ],
      "redditUsername": null,
      "ircNick": "Xaiier",
      "description": "",
      "forumUsername": "Xaiier"
    }
```
</details>



<!-- Missing: Change User Password  -->
<!-- Missing: Delete User  -->
<!-- Missing: Update User Background  -->
<br />

## Mods

You can query the API for information on a specific mod, a specific version, and
so on. This could be useful, for example, to implement an update checker. You can
also use the API to create new mods or update existing ones.

### GET /api/mod/\<mod_id>

`GET /api/mod/\<mod_id>`

Returns information about a specific mod.

*Curl*
```sh
curl "https://spacedock.info/api/mod/21"
```

<details><summary><i>Example Response</i></summary>

```json
{
  "downloads": 332,
  "name": "Time Control",
  "followers": 19,
  "author": "Xaiier",
  "default_version_id": 371,
  "versions": [
    {
      "changelog": "...",
      "game_version": "0.24.2",
      "download_path": "/mod/21/Time%20Control/download/13.0",
      "id": 371,
      "friendly_version": "13.0"
    }
  ],
  "background": "...",
  "bg_offset_y": 1234,
  "description:" "...markdown...",
  "description_html": "...html...",
  "id": 21,
  "short_description": "...",
  "updated": "...date/time..."
}
```
</details>

<br />

### GET /api/mod/\<mod_id>/latest

`GET /api/mod/\<mod_id>/latest` 

<!-- TODO: Need to change this to get mod latest Version -->

Returns the latest version of a mod.

*Curl*
```sh
curl "https://spacedock.info/api/mod/21/latest"
```

<details><summary><i>Example Response</i></summary>

```json
{
  "changelog": "...",
  "game_version": "0.24.2",
  "download_path": "/mod/21/Time%20Control/download/13.0",
  "id": 371,
  "friendly_version": "13.0"
}
```
</details>

<br />

### POST /api/mod/create

Creates a new mod. **Requires authentication**.

*Curl*
```sh
    curl -b ./cookies \
        -F "name=Example Mod" \
        -F "short-description=this is your schort description" \
        -F "version=1.0" \
        -F "game-short-name=kerbal-space-program" \
        -F "game-version=0.24" \
        -F "license=GPLv2" \
        -F "zipball=@ExampleMod.zip" \
        "https://spacedock.info/api/mod/create"
```

*Parameters (Form Data)*

* `name`: Your new mod's name
* `short-description`: Short description of your mod
* `version`: The latest friendly version of your mod
* `game-short-name`: The short name of the game your mod is for. Alternatively specify the id with `game-id`.
* `game-version`: The game version this is compatible with
* `license`: Your mod's license
* `zipball`: The actual mod's zip file
* `notifications`: List of ids of notifications to enable (use **/api/&lt;gameid&gt;/notifications** to get available options)

<details><summary><i>Example Response</i></summary>

```json
{
  "url": "/mod/1234/Example Mod"
}
```
</details>

*Notes*

This creates an unpublished mod. You must log into the actual site to publish
your mod.

<br />

### POST /api/mod/\<mod_id>/update

Publishes an update to an existing mod. **Requires authentication**.

*Curl*
```sh
    curl -b ./cookies \
        -F "version=1.0" \
        -F "changelog=this is your changelog" \
        -F "game-version=0.24" \
        -F "notify-followers=yes" \
        -F "zipball=@ExampleMod.zip" \
        "https://spacedock.info/api/mod/1234/update"
```

*Parameters (Form Data)*

* `version`: The friendly version number about to be created
* `changelog`: Markdown changelog
* `game-version`: The game version this is compatible with
* `notify-followers`: If "yes", email followers about this update
* `zipball`: The actual mod's zip file

<!-- MISSING: Get Mod KSP AVC -->
<!-- MISSING: Update Mod BG -->
<!-- MISSING: Grant Mod Access -->
<!-- MISSING: Accept Mod Grant -->
<!-- MISSING: Reject mod Grant -->
<!-- MISSING: Set Mod Default Version -->
<!-- MISSING: Update Mod Edit Version -->
<br />

## Games

### GET /api/kspversions

This is deprecated. Use **/api/games** to find the ID of a game, then **/api/&lt;gameid&gt;/versions** to get its versions.
<br />

### GET /api/games

This will list the available games and their ids.

*Curl*
```sh
    curl "https://spacedock.info/api/games
```

<details><summary><i>Example Response</i></summary>

```json
[
  {
    "id": 1,
    "name": "Kerbal Space Program",
    "publisher_id": 1,
    "short_description": null,
    "description": null,
    "created": "2019-07-16T02:34:58.756291",
    "background": null,
    "bg_offset_x": null,
    "bg_offset_y": null,
    "link": null
  }
]
```
</details>

<br />

### GET /api/\<game_id>/versions

This will list the available versions of a game.
For KSP the response is the same as `/api/kspversions`

*Curl*
```sh
    curl "https://spacedock.info/api/\<gameid>/versions"
```

<details><summary><i>Example Response</i></summary>

```json
[
  {
    "id": 170,
    "friendly_version": "1.9.1"
  },
  {
    "id": 169,
    "friendly_version": "1.9.0"
  },
  ...continued...
]
```
</details>

<br />

### GET /api/\<game_id>/notifications

Returns the notifications that can be enabled for mods from this game.

*Curl*
```sh
curl "https://spacedock.info/api/\<gameid>/notifications"
```

<details><summary><i>Example Response</i></summary>

```json
[
  {
    "id": 1,
    "name": "CKAN",
    "builds_url": "https://github.com/KSP-CKAN/CKAN-meta/raw/master/builds.json",
    "add_url": "https://netkan.ksp-ckan.space/sd/add/ksp",
    "change_url": "https://netkan.ksp-ckan.space/sd/inflate/ksp"
  }
]
```
</details>

<br />

### POST /api/download_counts

This will return download counts for the specified mods.

*Curl*
```sh
    curl -d 'mod_id=1&mod_id=2&mod_id=3' https://spacedock.info/api/download_counts
```

<details><summary><i>Example Response</i></summary>

```json
{
  "download_counts": [
    {
      "id": 1,
      "downloads": 53
    },
    {
      "id": 2,
      "downloads": 2
    },
    {
      "id": 3,
      "downloads": 1
    }
  ]
}
```
</details>


<br />
