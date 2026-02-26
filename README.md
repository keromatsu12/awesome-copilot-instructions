# Standard Development Guidelines & Copilot Instructions

[**日本語 (Japanese)**](./README.ja.md)

## Overview

This repository defines the development standards, architectural guidelines, and Copilot instructions to be followed in the project. It serves as a comprehensive reference for maintaining code quality, consistency, and scalability across the application.

By adhering to these guidelines, developers and AI assistants (specifically GitHub Copilot) can work in alignment with the project's core principles.

## Directory Structure

The repository is organized to provide clear separation between general instructions and specific technical skills.

### `.github/`

This directory contains the core configuration and documentation for development workflows.

- **`copilot-instructions.md`**
  - **Description:** A set of custom instructions tailored for GitHub Copilot.
  - **Purpose:** Ensures that AI-generated code adheres to the project's specific coding styles, architectural patterns (Clean Architecture), and testing requirements. It acts as a high-level skeleton that references detailed skill documents.

- **`skills/`**
  - **Description:** A collection of detailed guides and standards categorized by technical domain. Each directory contains a `SKILL.md` file explaining the domain in depth.
  - **Contents:**
    - **`architecture/`**: Defines the Clean Architecture layers (Domain, Application, Presentation, Infrastructure) and dependency rules.
    - **`backend-integration/`**: Guidelines for API integration, authentication handling, and data synchronization.
    - **`coding-standards/`**: Enforces naming conventions, code formatting, and best practices (SOLID, DRY, KISS).
    - **`state-management/`**: Standards for managing global and local state (e.g., Zustand, TanStack Query).
    - **`testing/`**: Strategies for unit and integration testing, including the AAA pattern and coverage requirements.
    - **`ui-design/`**: Guidelines for building responsive UIs, styling conventions (CSS/BEM), and component structure.

## Usage

1.  **For Developers:** Review the `skills/` directory relevant to your current task before starting implementation.
2.  **For AI Assistance:** Ensure `copilot-instructions.md` is referenced or active in your editor context to receive accurate code suggestions.
