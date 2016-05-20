# GitLab to Atlassian

This repository contains two scripts (`dump_gitlab_json.py` and
`gitlab_to_stash.py`) that export issues and code from GitLab to JIRA and Stash
respectively. Each script is described in more detail below.

***

## Dumping JIRA-compatible issue JSON from GitLab

This script attempts to export all of the issues (and the users associated with
them) from GitLab to JIRA. I wrote it in a few hours, but it got the job done.
I'm posting it on GitHub since other people may also find it useful.

### Usage

```
usage: dump_gitlab_json.py [-h] [-c] [-d DATE_FILTER]
                           [-D DEFAULT_JIRA_PROJECT] [-e] [-i IGNORE_LIST]
                           [-I INCLUDE_LIST [INCLUDE_LIST ...]] [-m]
                           [-M PROJECT_MAP [PROJECT_MAP ...]] [-n]
                           [-p PASSWORD] [-P PAGE_SIZE] [-s]
                           [-S STATUS_MAP [STATUS_MAP ...]] [-t TOKEN]
                           [-T ISSUE_TYPE] [-u USERNAME] [-v] [--version]
                           gitlab_url

Export all users/issues from GitLab to JIRA JSON format.

positional arguments:
  gitlab_url            The full URL to your GitLab instance.

optional arguments:
  -h, --help            show this help message and exit
  -c, --projects_to_components
                        Create JIRA components from GitLab project names.
                        (default: False)
  -d DATE_FILTER, --date_filter DATE_FILTER
                        Only include issues, notes, etc. created after the
                        specified date. Expected format is YYYY-MM-DD
                        (default: 1970-01-01)
  -D DEFAULT_JIRA_PROJECT, --default_JIRA_project DEFAULT_JIRA_PROJECT
                        Optional default JIRA project name for all project
                        issues to be imported into (default: None)
  -e, --include_empty   Include projects in output that do not have any
                        issues. (default: False)
  -i IGNORE_LIST, --ignore_list IGNORE_LIST
                        List of project names to exclude from dump. (Read from
                        a file, one project name per line.) (default: None)
  -I INCLUDE_LIST [INCLUDE_LIST ...], --include_list INCLUDE_LIST [INCLUDE_LIST ...]
                        List of project names to include in dump. (Read as
                        space delimited arguments.) (default: None)
  -m, --preserve_markdown
                        Do not convert GitLab MarkDown to JIRA Wiki markup.
                        (default: False)
  -M PROJECT_MAP [PROJECT_MAP ...], --project_map PROJECT_MAP [PROJECT_MAP ...]
                        Map of GL project names to thier corresponding JIRA
                        project names. (Read as space delimited arguments of
                        the form oldprojname=newprojname.) (default: None)
  -n, --add_GL_namespace_to_ID
                        Add the GL namespace to the external JIRA ID. This is
                        necessary if importing multiple GL projects into a
                        single JIRA project. (default: False)
  -p PASSWORD, --password PASSWORD
                        The password to use to authenticate if token is not
                        specified. If password and token are both unspecified,
                        you will be prompted to enter a password. (default:
                        None)
  -P PAGE_SIZE, --page_size PAGE_SIZE
                        When retrieving result from GitLab, how many results
                        should be included in a given page?. (default: 20)
  -s, --verify_ssl      Enable SSL certificate verification (default: False)
  -S STATUS_MAP [STATUS_MAP ...], --status_map STATUS_MAP [STATUS_MAP ...]
                        Map of GL project statuses to thier corresponding JIRA
                        statuses. (Read as space delimited arguments of the
                        form oldstatus=newstatus.) (default: None)
  -t TOKEN, --token TOKEN
                        The private GitLab API token to use for
                        authentication. Either this or username and password
                        must be set. (default: None)
  -T ISSUE_TYPE, --issue_type ISSUE_TYPE
                        Specify the default JIRA issue type. (default: Bug)
  -u USERNAME, --username USERNAME
                        The username to use for authentication, if token is
                        unspecified. (default: None)
  -v, --verbose         Print more status information. For every additional
                        time this flag is specified, output gets more verbose.
                        (default: 0)
  --version             show program's version number and exit
```

***

## Exporting GitLab projects and repositories to Stash

`gitlab_to_stash.py` clones all projects from GitLab and recreates them on
Stash.  It attempts to replicate the GitLab's project/namespace hierarchy.

### Usage

```
usage: gitlab_to_stash.py [-h] [-p PASSWORD] [-P PAGE_SIZE] [-s] [-S]
                          [-t TOKEN] [-u USERNAME] [-v] [--version]
                          gitlab_url stash_url

Transfer all projects/repositories from GitLab to Stash. Note: This script
assumes you have your SSH key registered with both GitLab and Stash.

positional arguments:
  gitlab_url            The full URL to your GitLab instance.
  stash_url             The full URL to your Stash instance.

optional arguments:
  -h, --help            show this help message and exit
  -p PASSWORD, --password PASSWORD
                        The password to use to authenticate if token is not
                        specified. If password and token are both unspecified,
                        you will be prompted to enter a password. (default:
                        None)
  -P PAGE_SIZE, --page_size PAGE_SIZE
                        When retrieving result from GitLab, how many results
                        should be included in a given page?. (default: 20)
  -s, --verify_ssl      Enable SSL certificate verification (default: False)
  -S, --skip_existing   Do not update existing repositories and just skip
                        them. (default: False)
  -t TOKEN, --token TOKEN
                        The private GitLab API token to use for
                        authentication. Either this or username and password
                        must be set. (default: None)
  -u USERNAME, --username USERNAME
                        The username to use for authentication, if token is
                        unspecified. (default: None)
  -v, --verbose         Print more status information. For every additional
                        time this flag is specified, output gets more verbose.
                        (default: 0)
  --version             show program's version number and exit
```

***

## Requirements

- Python 3 (although pull request to support 2 are welcome)
- [Python GitLab API library](https://github.com/Itxaka/pyapi-gitlab)
- [Stashy](https://github.com/RisingOak/stashy) Python library
- Admin access to GitLab is necessary to export private repositories

## License

New BSD License (3-clause)
