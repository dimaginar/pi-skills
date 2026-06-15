# pi-skills

A collection of skills for the [Pi coding agent](https://github.com/earendil-works/pi).

## What are skills?

Skills are markdown files that instruct Pi how to handle specific tasks: bash commands, workflows, and analysis prompts combined into reusable building blocks.

## Usage

Copy the full skill directory (e.g. `skills/aur-update-checker/`) into your project's skills folder and reference it in your Pi config. Each skill has its own README with specific setup instructions.

## Skills

| Skill | Description |
|-------|-------------|
| [aur-update-checker](skills/aur-update-checker/) | Detect AUR package updates and security-analyze PKGBUILDs before installing |

## Requirements

- [Pi coding agent](https://github.com/earendil-works/pi)
- Skill-specific requirements listed in each skill's README