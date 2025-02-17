
# Change Log
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/)
and this project adheres to [Semantic Versioning](http://semver.org/).

## [0.8.0] — 2024-06-25

### Added

- New Settings page to change Dawarich settings.
- New "Fog of War" toggle on the map controls.
- New "Fog of War meters" field in Settings. This field allows you to set the radius in meters around the point to be shown on the map. The map outside of this radius will be covered with a fog of war.

### Changed

- Order of points on Points page is now descending by timestamp instead of ascending.

---

## [0.7.1] — 2024-06-20

In new Settings page you can now change the following settings:

- Maximum distance between two points to consider them as one route
- Maximum time between two points to consider them as one route

### Added

- New Settings page to change Dawarich settings.

### Changed

- Settings link in user menu now redirects to the new Settings page.
- Old settings page is now available undeer Account link in user menu.

---

## [0.7.0] — 2024-06-19

## The GPX MVP Release

This release introduces support for GPX files to be imported. Now you can import GPX files from your devices to Dawarich. The import process is the same as for other kinds of files, just select the GPX file instead and choose "gpx" as a source. Both single-segmented and multi-segmented GPX files are supported.

⚠️ BREAKING CHANGES: ⚠️

- `/api/v1/points` endpoint is removed. Please use `/api/v1/owntracks/points` endpoint to upload your points from OwnTracks mobile app instead.

### Added

- Support for GPX files to be imported.

### Changed

- Couple of unnecessary params were hidden from route popup and now can be shown using `?debug=true` query parameter. This is useful for debugging purposes.

### Removed

- `/exports/download` endpoint is removed. Now you can download your exports directly from the Exports page.
- `/api/v1/points` endpoint is removed.

---

## [0.6.4] — 2024-06-18

### Added

- A link to Dawarich's website in the footer. It ain't much, but it's honest work.

### Fixed

- Fixed version badge in the navbar. Now it will show the correct version of the application.

### Changed

- Default map center location was changed.

---

## [0.6.3] — 2024-06-14

⚠️ IMPORTANT: ⚠️

Please update your `docker-compose.yml` file to include the following changes:

```diff
  dawarich_sidekiq:
    image: freikin/dawarich:latest
    container_name: dawarich_sidekiq
    volumes:
      - gem_cache:/usr/local/bundle/gems
+     - public:/var/app/public
```

### Added

- Added a line with public volume to sidekiq's docker-compose service to allow sidekiq process to write to the public folder

### Fixed

- Fixed a bug where the export file was not being created in the public folder

---

## [0.6.2] — 2024-06-14

This is a debugging release. No changes were made to the application.

---

## [0.6.0] — 2024-06-12

### Added

- Exports page to list existing exports download them or delete them

### Changed

- Exporting process now is done in the background, so user can close the browser tab and come back later to download the file. The status of the export can be checked on the Exports page.

ℹ️ Deleting Export file will only delete the file, not the points in the database. ℹ️

⚠️ BREAKING CHANGES: ⚠️

Volume, exposed to the host machine for placing files to import was changed. See the changes below.

Path for placing files to import was changed from `tmp/imports` to `public/imports`.

```diff
  ...

  dawarich_app:
    image: freikin/dawarich:latest
    container_name: dawarich_app
    volumes:
      - gem_cache:/usr/local/bundle/gems
-     - tmp:/var/app/tmp
+     - public:/var/app/public/imports

  ...
```

```diff
  ...

volumes:
  db_data:
  gem_cache:
  shared_data:
- tmp:
+ public:
```

---

## [0.5.3] — 2024-06-10

### Added

- A data migration to remove points with 0.0, 0.0 coordinates. This is necessary to prevent errors when calculating distance in Stats page.

### Fixed

- Reworked code responsible for importing "Records.json" file from Google Takeout. Now it is more reliable and faster, and should not throw as many errors as before.

---

## [0.5.2] — 2024-06-08

### Added

- Test version of google takeout importing service for exports from users' phones

---

## [0.5.1] — 2024-06-07

### Added

- Background jobs concurrency now can be set with `BACKGROUND_PROCESSING_CONCURRENCY` env variable in `docker-compose.yml` file. Default value is 10.
- Hand-made favicon

### Changed

- Change minutes to days and hours on route popup

### Fixed

- Improved speed of "Stats" page loading by removing unnecessary queries

---

## [0.5.0] — 2024-05-31

### Added

- New buttons to quickly move to today's, yesterday's and 7 days data on the map
- "Download JSON" button to points page
- For debugging purposes, now user can use `?meters_between_routes=500` and `?minutes_between_routes=60` query parameters to set the distance and time between routes to split them on the map. This is useful to understand why routes might not be connected on the map.
- Added scale indicator to the map

### Changed

- Removed "Your data" page as its function was replaced by "Download JSON" button on the points page
- Hovering over a route now also shows time and distance to next route as well as time and distance to previous route. This allows user to understand why routes might not be connected on the map.

---

## [0.4.3] — 2024-05-30

### Added

- Now user can hover on a route and see when it started, when it ended and how much time it took to travel

### Fixed

- Timestamps in export form are now correctly assigned from the first and last points tracked by the user
- Routes are now being split based both on distance and time. If the time between two consecutive points is more than 60 minutes, the route is split into two separate routes. This improves visibility of the routes on the map.

---

## [0.4.2] — 2024-05-29

### Changed

- Routes are now being split into separate one. If distance between two consecutive points is more than 500 meters, the route is split into two separate routes. This improves visibility of the routes on the map.
- Background jobs concurrency is increased from 5 to 10 to speed up the processing of the points.

### Fixed

- Point data, accepted from OwnTracks and Overland, is now being checked for duplicates. If a point with the same timestamp and coordinates already exists in the database, it will not be saved.

---
## [0.4.1] — 2024-05-25

### Added

- Heatmap layer on the map to show the density of points

---

## [0.4.0] — 2024-05-25

**BREAKING CHANGES**:

- `/api/v1/points` is still working, but will be **deprecated** in nearest future. Please use `/api/v1/owntracks/points` instead.
- All existing points recorded directly to the database via Owntracks or Overland will be attached to the user with id 1.

### Added

- Each user now have an api key, which is required to make requests to the API. You can find your api key in your profile settings.
- You can re-generate your api key in your profile settings.
- In your user profile settings you can now see the instructions on how to use the API with your api key for both OwnTracks and Overland.
- Added docs on how to use the API with your api key. Refer to `/api-docs` for more information.
- `POST /api/v1/owntracks/points` endpoint.
- Points are now being attached to a user directly, so you can only see your own points and no other users of your applications can see your points.

### Changed

- `/api/v1/overland/batches` endpoint now requires an api key to be passed in the url. You can find your api key in your profile settings.
- All existing points recorded directly to the database will be attached to the user with id 1.
- All stats and maps are now being calculated and rendered based on the user's points only.
- Default `TIME_ZONE` environment variable is now set to 'UTC' in the `docker-compose.yml` file.

### Fixed

- Fixed a bug where marker on the map was rendering timestamp without considering the timezone.

---

## [0.3.2] — 2024-05-23

### Added

- Docker volume for importing Google Takeout data to the application

### Changed

- Instruction on how to import Google Takeout data to the application

---

## [0.3.1] — 2024-05-23

### Added

- Instruction on how to import Google Takeout data to the application

---

## [0.3.0] — 2024-05-23

### Added

- Add Points page to display all the points as a table with pagination to allow users to delete points
- Sidekiq web interface to monitor background jobs is now available at `/sidekiq`
- Now you can choose a date range of points to be exported

---

## [0.2.6] — 2024-05-23

### Fixed

- Stop selecting `raw_data` column during requests to `imports` and `points` tables to improve performance.

### Changed

- Rename PointsController to MapController along with all the views and routes

### Added

- Add Points page to display all the points as a table with pagination to allow users to delete points

---

## [0.2.5] — 2024-05-21

### Fixed

- Stop ignoring `raw_data` column during requests to `imports` and `points` tables. This was preventing points from being created.

---

## [0.2.4] — 2024-05-19

### Added

- In right sidebar you can now see the total amount of geopoints aside of kilometers traveled

### Fixed

- Improved overall performance if the application by ignoring `raw_data` column during requests to `imports` and `points` tables.

---


## [0.2.3] — 2024-05-18

### Added

- Now you can import `records.json` file from your Google Takeout archive, not just Semantic History Location JSON files. The import process is the same as for Semantic History Location JSON files, just select the `records.json` file instead and choose "google_records" as a source.

---


## [0.2.2] — 2024-05-18

### Added

- Swagger docs, can be found at `https:<your-host>/api-docs`

---

## [0.2.1] — 2024-05-18

### Added

- Cities, visited by user and listed in right sidebar now also have an active link to a date they were visited

### Fixed

- Dark/light theme switcher in navbar is now being saved in user settings, so it persists between sessions

---

## [0.2.0] — 2024-05-05

*Breaking changes:*

This release changes how Dawarich handles a city visit threshold. Previously, the `MINIMUM_POINTS_IN_CITY` environment variable was used to determine the minimum *number of points* in a city to consider it as visited. Now, the `MIN_MINUTES_SPENT_IN_CITY` environment variable is used to determine the minimum *minutes* between two points to consider them as visited the same city.

The logic behind this is the following: if you have a lot of points in a city, it doesn't mean you've spent a lot of time there, especially if your OwnTracks app was in "Move" mode. So, it's better to consider the time spent in a city rather than the number of points.

In your docker-compose.yml file, you need to replace the `MINIMUM_POINTS_IN_CITY` environment variable with `MIN_MINUTES_SPENT_IN_CITY`. The default value is `60`, in minutes.

---

## [0.1.9] — 2024-04-25

### Added

- A test for CheckAppVersion service class

### Changed

- Replaced ActiveStorage with Shrine for file uploads

### Fixed

- `ActiveStorage::FileNotFoundError` error when uploading export files

---

## [0.1.8.1] — 2024-04-21

### Changed

- Set Redis as default cache store

### Fixed

- Consider timezone when parsing datetime params in points controller
- Add rescue for check version service class

---

## [0.1.8] — 2024-04-21

### Added

- Application version badge to the navbar with check for updates button
- Npm dependencies install to Github build workflow
- Footer

### Changed

- Disabled map points rendering by default to improve performance on big datasets

---

## [0.1.7] — 2024-04-17

### Added

- Map controls to toggle polylines and points visibility

### Changed

- Added content padding for mobile view
- Fixed stat card layout for mobile view

---

## [0.1.6.3] — 2024-04-07

### Changed

- Removed strong_params from POST /api/v1/points

---

## [0.1.6.1] — 2024-04-06

### Fixed

- `ActiveStorage::FileNotFoundError: ActiveStorage::FileNotFoundError` error when uploading export files

---

## [0.1.6] — 2024-04-06

You can now use [Overland](https://overland.p3k.app/) mobile app to track your location.

### Added

- Overland API endpoint (POST /api/v1/overland/batches)

### Changed

### Fixed

---

## [0.1.5] — 2024-04-05

You can now specify the host of the application by setting the `APPLICATION_HOST` environment variable in the `docker-compose.yml` file.

### Added

- Added version badge to navbar
- Added APPLICATION_HOST environment variable to docker-compose.yml to allow user to specify the host of the application
- Added CHANGELOG.md to keep track of changes

### Changed

- Specified gem version in Docker entrypoint

### Fixed
