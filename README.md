# Beehive HCM Attendance Automation

A documentation and automation-planning repository for submitting attendance through a Beehive HCM
portal while handling OTP-based authentication and workplace IT constraints responsibly.

## Overview

Research and delivery-package repository. It contains feasibility analysis, product requirements,
Tasker-oriented assets, and implementation notes rather than a general-purpose attendance product.

## What This Repository Contains

- Feasibility and PRD documents under `docs/`.
- Tasker delivery assets under `tasker/`.
- A delivery-package summary for handoff and usage planning.

## Who This Is For

- Automation practitioners
- Android Tasker users
- Maintainers evaluating attendance-submission options

## Repository Structure

| Path | Purpose |
|------|---------|
| `docs/` | Feasibility, PRD, and implementation documentation. |
| `tasker/` | Android Tasker profile/task assets. |
| `DELIVERY-PACKAGE.md` | Packaged delivery notes. |
| `README.md` | Project overview and usage guide. |

## Getting Started

- Read the feasibility analysis before using any automation.
- For Tasker-based use, import the Tasker assets on a controlled Android device and verify the OTP flow manually.

## Common Workflows

- Test automation only in an authorized environment.
- Keep manual fallback steps documented for days when OTP, network, or portal changes break automation.

## Quality, Security, And Maintenance Notes

- Do not bypass employer policy, authentication controls, or consent requirements.
- Avoid storing OTPs, credentials, or workplace-sensitive data in tracked files.

## Current Documentation State

This README was rewritten to make the repository purpose, structure, setup path, and safety
expectations clear to a new reader. If implementation details change, update this file in the same
change so the GitHub landing page stays accurate.

Last documentation refresh: 2026-05-31.
