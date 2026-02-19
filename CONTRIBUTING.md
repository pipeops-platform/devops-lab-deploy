# Contributing Guide

Thank you for contributing to the DevOps Hybrid Lab.

## Branching Model

- Use short-lived feature branches from `main`
- Open pull requests for all changes
- Keep branches focused on one concern

## Commit Messages

Use clear, action-oriented commits.

Suggested prefixes:

- `feat:` new functionality
- `fix:` bug fix
- `docs:` documentation updates
- `chore:` maintenance tasks
- `refactor:` internal improvements

## Pull Request Expectations

- Link to relevant issue/task when applicable
- Keep scope small and reviewable
- Include deployment validation evidence
- Update docs when manifest behavior changes

## Quality Checks

Before opening a PR:

1. Validate manifest syntax and structure
2. Validate target overlay changes
3. Validate GitOps compatibility (ArgoCD expected behavior)

## Security and Secrets

- Do not commit plaintext secrets
- Prefer external secret management patterns
- Avoid manual cluster drift changes
