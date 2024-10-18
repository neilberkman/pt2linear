# Pivotal Tracker to Linear Migration Script

This script facilitates the migration of data from Pivotal Tracker to Linear. It's a hacky but functional solution that might be useful for teams moving to Linear, especially given Pivotal Tracker's impending shutdown.

Linear does offer an [official CLI importer tool](https://github.com/linear/linear/tree/master/packages/import) that supports Pivotal Tracker. However, it's quite limited in its functionality, which is why this more comprehensive script was developed.

## Features

- Migrates Pivotal Tracker epics to Linear projects
- Converts Pivotal Tracker stories to Linear issues
- Preserves attachments, including inline display for images
- Links issues to their corresponding projects (based on PT epic-story relationships)
- Retains labels
- Maintains story/issue state mapping
- Preserves comments and tasks

## Setup

1. Clone this repository
2. Install required gems: `bundle install`
3. Copy `.env.sample` to `.env` and fill in your API tokens and other required information
4. Set up the MimeMagic dependency (see note below)

**Note on MimeMagic**: This script uses the MimeMagic gem, which requires additional setup. Please follow the instructions in the [MimeMagic README](https://github.com/mimemagicrb/mimemagic?tab=readme-ov-file#dependencies) to ensure you have the necessary dependencies installed.

## Usage

```
./pt2linear.rb [options]
```

Options:
- `--migrate`: Perform the migration
- `--assign`: Assign unassigned issues (use after migration)
- `--dry-run`: Perform a dry run without making changes
- `-v, --verbose`: Run with verbose logging

## Important Notes

- Ensure the `LINEAR_TIMEZONE` in your `.env` file matches the timezone set in your Linear account. This is crucial for proper rate limit handling.
- The script uses rate limiting to avoid overwhelming the Linear API. Adjust sleep durations if needed.
- This script creates a "migrated_story" label in Linear for all migrated issues.

## Team Recreation Script

Included in this repository is a `recreate_team` script that allows you to delete and recreate a Linear team. This can be useful if you're just getting set up and aren't actively using Linear yet, as it allows you to easily modify the import script and rerun the whole migration process.

### Usage:
- `./recreate_team` for interactive mode (requires typing "delete team" to confirm)
- `./recreate_team -f` to force deletion without confirmation

⚠️ **WARNING: This script is very dangerous!** It will delete all data in the specified Linear team. Only use this if you're absolutely sure you want to start over and don't mind losing all existing data in the team.

## Possible Improvement

- Add links to Pull Requests instead of including them in the description.

## Disclaimer

This script is provided as-is. It's recommended to run it on a test project first and verify the results before using it on your main project data.