# Trira

A tool that is able to convert cards in Trello board into JIRA. It creates JIRA Tasks based on content of cards and link them to
provided Epic.

## Prerequisites

* Node.js v6 or newer
* Trello API key and [Trello token](https://developers.trello.com/get-started/start-building)
* JIRA instance with Greenhopper Epic support
* JIRA user and password or JIRA instance with GSS API (Kerberos) enabled and active ticket on your system

## Usage


First, create an epic in JIRA that contain correct metadata, namely _Fix Version_. The value will be copied to all issues.

Afterwards install the tool via

```
npm install -g trira
```

Or, use development copy by
```
git clone https://github.com/kpiwko/trira.git && cd trira && npm install && npm link
```

First, provide credentials for Trello and JIRA. JIRA credentials are optional, you can also use a Kerberos Ticket for authentication (via `--gss-api` parameter)- if your JIRA instance supports that. For example, a JIRA host could be _issues.jboss.org_.
```
trira target <jiraHost> --trello-key=<trello-key> --trello-token=<trello-token> --jira-user=<jira-user> --jira-password=<jira-password> [--strict-ssl=true|false] [--gss-api=true|false]
```

Afterwards, you run:
```
trira sync <trello-board-regexp> <jira-epic-name>
```

There are further options available, such as which Trello columns will be synced or a card name regular expression for more granular filtering. 
For more details about command usage, please run:
```
trira help sync
```

## Assumptions

* All checklists are included in JIRA description field
* Labels on cards in Trello are used to label issues in JIRA (labels in Trello can't contain spaces)
* It is current not possible to sync data from JIRA to Trello
* Tool ignores existing issues, hence every run creates new issues - instead of updating them
* Tool creates issues where provided jira user acts as reporter
* Story points are represented in Trello card name - in the beginning as a number in parentheses, such as (3)

## Kerberos related testing

1. Build docker image - check for `$USER` value if you want easier Kerberos ticket request
    docker build -t trira-centos .
2. From within image, configure Kerberos KDC and ask for ticket
    docker run -v `pwd`:/trira -it trira-centos
3. Continue with development. If your OS is different, make sure you wipe current _node_modules_


