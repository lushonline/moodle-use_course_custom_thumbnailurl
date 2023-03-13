# Moodle modification to support an external thumbnail images on courses

A modification to allow course thumbnail images to be delivered from a URL external to Moodle, for example from a CDN.

The URL is configured as a [Course Custom Field](https://docs.moodle.org/39/en/Course_settings#Course_custom_fields)

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [How it works](#how-it-works)

## Requirements

To use this plugin you will need:

1. You must add a [Course Custom Field](https://docs.moodle.org/39/en/Course_settings#Course_custom_fields) with a shortname **thumbnailurl**
1. You must populate this field with an absolute URL to the thumbnail image.

## Installation

1. Copy the file from the appropriate folder to your Moodle Version
1. Create the custom field and populate for your courses

## How it works

The modification works by updating the logic that retrieves the course thumbnail from the course overview files.

### MOODLE_39_STABLE

The [/course/classes/external/course_summary_exporter.php](https://github.com/moodle/moodle/blob/MOODLE_39_STABLE/course/classes/external/course_summary_exporter.php) is modified.

The [get_course_image function](https://github.com/moodle/moodle/blob/MOODLE_39_STABLE/course/classes/external/course_summary_exporter.php#L170) is modified so it calls two new private funtions.

1. get_image_url_from_overview_files - This function checks if a course overview image was uploaded, and if it was it uses this.
1. get_image_url_from_customfield - This function is called if get_image_url_from_overview_files returns null, this looks for the custom field and if populated returns this url.

### MOODLE_311_STABLE and later

These versions of Moodle introduce a cache lookup of the courseimage, so we modified the [/course/classes/cache/course_image.php](https://github.com/moodle/moodle/blob/MOODLE_311_STABLE/course/classes/cache/course_image.php)

The [load_for_cache function](https://github.com/moodle/moodle/blob/MOODLE_311_STABLE/course/classes/cache/course_image.php#L58) is modified to use the same logic as for Moodle 3.9

## License

2019-2023 LushOnline

This program is free software: you can redistribute it and/or modify it under
the terms of the GNU General Public License as published by the Free Software
Foundation, either version 3 of the License, or (at your option) any later
version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY
WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A
PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with
this program. If not, see <http://www.gnu.org/licenses/>.

