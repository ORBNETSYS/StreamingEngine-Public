# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),

## [2.9.2] - 06.03.2022

- Added: Timelapse monitoring in case of broken frame grabbers
- Changed: License and Config files and Service logs are now located in ProgramData instead of AppData
- Changed: Installing Milestone plugins will copy files to Management Client and Event Server MIP Plugins folder
- Changed: Log level must be set to Debug to view timelapse saving logs
- Fix: Changing account login of the Streaming Engine Service will not break anything.
- Fix: Installing Streaming Engine Service to other driver than C:\ drive is now supported
- Fix: License Unique ID will be set as soon as first connection to Milestone is established.

