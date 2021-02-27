Nextcloud Ansible Role
======================

![GitHub](https://img.shields.io/github/license/freiraumpetersburg/ansible-nextcloud?style=flat-square)
![GitHub release](https://img.shields.io/github/v/release/freiraumpetersburg/ansible-nextcloud?style=flat-square)

Ansible role for [Nextcloud](https://nextcloud.com/), a cloud service with many features and extensions.

Nextcloud is run inside a Docker container, on top of a Debian base.

This role was originally developed for the [Kulturverein Petersburg](https://kulturverein-petersburg.de/), but is now deployed in several different places.


Update
------

To bump the Nextcloud version, change the `nextcloud_version` variable.

To update the underlying container (e.g. to update the php version), use

```bash
ansible-playbook PLAYBOOK.yml -i hosts/production --ask-vault-pass --tags="nextcloud" --extra-vars "nextcloud_force_rebuild=true"
```

Settings
--------

### Ansible

- ansible settings overwrite manual settings
- they are located in `templates/*.config.php.j2`

### Groups

TODO

### Groupfolder

TODO

### Quota

TODO

### Share

TODO

### Theming

Check `defaults/main.yml` under `theming` for everything that is configurable.

The following currently cannot be changed via ansible, but can be configured through the Nextcloud admin interface:

- Logo
- Link to imprint
- Link to privacy statement
- Favicon

### Terms of use

TODO

### Ldap

You can enable LDAP support through the `nextcloud_ldap_enable` variable.

| Configuration                   | s01                                                                                                                                                 |
|---------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------|
| `hasMemberOfFilterSupport`      | `1`                                                                                                                                                 |
| `homeFolderNamingRule`          |                                                                                                                                                     |
| `lastJpegPhotoLookup`           | `0`                                                                                                                                                 |
| `ldapAgentName`                 | `cn=nextcloud,ou=admins,dc=petersburg,dc=test`                                                                                                      |
| `ldapAgentPassword`             | `***`                                                                                                                                               |
| `ldapAttributesForGroupSearch`  |                                                                                                                                                     |
| `ldapAttributesForUserSearch`   | <code>(&(&#124;(objectclass=inetOrgPerson))(&#124;(memberof=cn=nextcloud,ou=services,dc=petersburg,dc=test)))</code>                                |
| `ldapBackupHost`                |                                                                                                                                                     |
| `ldapBackupPort`                |                                                                                                                                                     |
| `ldapBase`                      | `dc=petersburg,dc=test`                                                                                                                             |
| `ldapBaseGroups`                | `ou=groups,dc=petersburg,dc=test`                                                                                                                   |
| `ldapBaseUsers`                 | `ou=people,dc=petersburg,dc=test`                                                                                                                   |
| `ldapCacheTTL`                  | `600`                                                                                                                                               |
| `ldapConfigurationActive`       | `1`                                                                                                                                                 |
| `ldapDefaultPPolicyDN`          | `cn=password,ou=policies,dc=petersburg,dc=test`                                                                                                     |
| `ldapDynamicGroupMemberURL`     |                                                                                                                                                     |
| `ldapEmailAttribute`            | `mail`                                                                                                                                              |
| `ldapExperiencedAdmin`          | `1`                                                                                                                                                 |
| `ldapExpertUUIDGroupAttr`       |                                                                                                                                                     |
| `ldapExpertUUIDUserAttr`        |                                                                                                                                                     |
| `ldapExpertUsernameAttr`        |                                                                                                                                                     |
| `ldapGidNumber`                 | `gidNumber`                                                                                                                                         |
| `ldapGroupDisplayName`          | `description`                                                                                                                                       |
| `ldapGroupFilter`               | <code>(&(&#124;(objectclass=groupOfUniqueNames)))</code>                                                                                            |
| `ldapGroupFilterGroups`         |                                                                                                                                                     |
| `ldapGroupFilterMode`           | `0`                                                                                                                                                 |
| `ldapGroupFilterObjectclass`    |                                                                                                                                                     |
| `ldapGroupMemberAssocAttr`      | `uniqueMember`                                                                                                                                      |
| `ldapHost`                      | `openldap`                                                                                                                                          |
| `ldapIgnoreNamingRules`         |                                                                                                                                                     |
| `ldapLoginFilter`               | <code>(&(objectClass=inetOrgPerson)(memberOf=cn=nextcloud,ou=services,dc=petersburg,dc=test)(&#124;(cn=%uid)(mail=%uid)(entryUUID=%uid)))</code>    |
| `ldapLoginFilterAttributes`     |                                                                                                                                                     |
| `ldapLoginFilterEmail`          | `0`                                                                                                                                                 |
| `ldapLoginFilterMode`           | `0`                                                                                                                                                 |
| `ldapLoginFilterUsername`       | `1`                                                                                                                                                 |
| `ldapNestedGroups`              | `0`                                                                                                                                                 |
| `ldapOverrideMainServer`        |                                                                                                                                                     |
| `ldapPagingSize`                | `500`                                                                                                                                               |
| `ldapPort`                      | `389`                                                                                                                                               |
| `ldapQuotaAttribute`            | `quota`                                                                                                                                             |
| `ldapQuotaDefault`              | `100 MB`                                                                                                                                            |
| `ldapTLS`                       | `0`                                                                                                                                                 |
| `ldapUserAvatarRule`            | `default`                                                                                                                                           |
| `ldapUserDisplayName`           | `displayname`                                                                                                                                       |
| `ldapUserDisplayName2`          |                                                                                                                                                     |
| `ldapUserFilter`                | `(&(objectClass=inetOrgPerson)(memberOf=cn=nextcloud,ou=services,dc=petersburg,dc=test))`                                                           |
| `ldapUserFilterGroups`          |                                                                                                                                                     |
| `ldapUserFilterMode`            | `0`                                                                                                                                                 |
| `ldapUserFilterObjectclass`     |                                                                                                                                                     |
| `ldapUuidGroupAttribute`        | `auto`                                                                                                                                              |
| `ldapUuidUserAttribute`         | `auto`                                                                                                                                              |
| `turnOffCertCheck`              | `1`                                                                                                                                                 |
| `turnOnPasswordChange`          | `1`                                                                                                                                                 |
| `useMemberOfToDetectMembership` | `1`                                                                                                                                                 |

Authentication is checked against cn or mail.


Maintainance
------------

### Diskspace estimate

      40 GB       Base
    + 20 GB * 0.5 Archive
    + 20 GB * 0.5 Trashbin
    --------
      60 GB


### Maintainance mode

Switch maintainance on:

    docker exec --user www-data nextcloud php occ maintenance:mode --on

Switch maintainance off:

    docker exec --user www-data nextcloud php occ maintenance:mode --off

### Restore

Generate new fingerprint after restore:

    docker exec --user www-data nextcloud php occ maintainance:data-fingerprint

### Nextcloud scan

Consult [nextcloud scan](https://scan.nextcloud.com).

### Open issues

- Emails of new user are public by default | [Ticket](https://github.com/nextcloud/server/issues/6582)
- There is actually no GPDR export | [Ticket](https://github.com/nextcloud/data_request/issues/17)


Privacy
-------

### Collection

- Communication data
    + IP (db: bruteforce_attempts, logs: nextcloud.log, audit.log)
    + Useragent (db: authtoken)
- Meta data
    + Activities
- User data
    + Profile
    + Files
    + Chats
    + Notes
    + Deck
    + Contacts
    + Calendar
    + Mail
    + Comments

### Measures

**IP**

- nextcloud logs (rotated logs are anonymized)
- nginx logs (instantly anonymized, deleted after 7 days)
- nextcloud db (bruteforce_attempts, TODO)

### Export

**Special tables**

- TODO
    + calendar_invitations
    + calendar_resources
    + calendar_rooms
    + circles_circles
    + circles_clouds
    + circles_groups
    + circles_links
    + circles_members
    + circles_shares
    + credentials
    + directlink
    + flow_checks
    + flow_operations
    + oauth2_access_tokens
    + oauth2_clients
    + polls_comments
    + polls_events
    + polls_notif
    + polls_options
    + polls_votes
    + richdocuments_assets
    + richdocuments_direct
    + richdocuments_member
    + share_external
- Temporary
    - activity_mq
    - filecache
    - talk_signaling
- No user data
    - announcements_groups
    - appconfig
    - deck_assigned_labels
    - deck_labels
    - e2e_encryption_lock
    - federated_reshares
    - file_locks
    - group_folders
    - group_folders_groups
    - group_folders_trash
    - groups
    - migrations
    - mimetypes
    - systemtag
    - systemtag_group
    - systemtag_object_mapping
    - talk_guests
    - talk_rooms
    - termsofservice_terms
    - trusted_servers
    - vcategory_to_object
    - whats_new

**Sql-Queries**

    SET @username = "<USERNAME>"
    SET @email = "user\@tld.test";

accounts

    SELECT * FROM accounts
    WHERE uid=@username

activity

    SELECT * FROM activity
    WHERE user=@username OR affecteduser=@username

addressbookchanges

    SELECT * FROM addressbookchanges
    INNER JOIN addressbooks ON addressbookchanges.addressbookid=addressbooks.id
    WHERE addressbooks.principaluri LIKE CONCAT('%', @username, '%')

addressbooks

    SELECT * FROM addressbooks
    WHERE principaluri LIKE CONCAT('%', @username, '%')

announcements

    SELECT * FROM announcements
    WHERE announcement_user=@username

authtoken

    SELECT * FROM authtoken
    WHERE uid=@username

bruteforce_attempts

    SELECT * FROM bruteforce_attempts
    WHERE metadata LIKE CONCAT('{"user":"%', @username, '%')
    OR metadata LIKE CONCAT('{"user":"%', @email, '%')

calendarchanges

    SELECT * FROM calendarchanges
    INNER JOIN calendars ON calendarchanges.calendarid=calendars.id
    WHERE calendars.principaluri LIKE CONCAT('%', @username, '%')

calendarobjects

    SELECT * FROM calendarobjects
    INNER JOIN calendars ON calendarobjects.calendarid=calendars.id
    WHERE calendars.principaluri LIKE CONCAT('%', @username, '%')

calendarobjects_props

    SELECT * FROM calendarobjects_props
    INNER JOIN calendars ON calendarobjects_props.calendarid=calendars.id
    WHERE calendars.principaluri LIKE CONCAT('%', @username, '%')

calendars

    SELECT * FROM calendars
    WHERE principaluri LIKE CONCAT('%', @username, '%')

calendarsubscriptions

    SELECT * FROM calendarsubscriptions
    WHERE principaluri LIKE CONCAT('%', @username, '%')

cards

    SELECT * FROM cards
    INNER JOIN addressbooks ON cards.addressbookid=addressbooks.id
    WHERE addressbooks.principaluri LIKE CONCAT('%', @username, '%')

cards_properties

    SELECT * FROM cards_properties
    INNER JOIN addressbooks ON cards_properties.addressbookid=addressbooks.id
    WHERE addressbooks.principaluri LIKE CONCAT('%', @username, '%')

comments

    SELECT * FROM comments
    WHERE actor_id=@username

comments_read_markers

    SELECT * FROM comments_read_markers
    WHERE user_id=@username

dav_shares

    SELECT * FROM dav_shares
    WHERE principaluri LIKE CONCAT('%', @username, '%')

deck_assigned_users

    SELECT * FROM deck_assigned_users
    WHERE participant=@username

deck_attachment

    SELECT * FROM deck_attachment
    WHERE created_by=@username

deck_board_acl

    SELECT * FROM deck_board_acl
    WHERE participant=@username

deck_boards

    SELECT * FROM deck_boards
    WHERE owner=@username

deck_cards

    SELECT * FROM deck_cards
    WHERE owner=@username OR last_editor=@username

deck_stacks

    SELECT * FROM deck_stacks
    INNER JOIN deck_boards ON deck_stacks.board_id=deck_boards.id
    WHERE deck_boards.owner=@username

files_trash

    SELECT * FROM files_trash
    WHERE user=@username

group_admin

    SELECT * FROM group_admin
    WHERE uid=@username

group_user

    SELECT * FROM group_user
    WHERE uid=@username

jobs

    SELECT * FROM jobs
    WHERE argument LIKE CONCAT('%"uid":"', @username, '%')

mail_accounts

    SELECT * FROM mail_accounts
    WHERE user_id=@username

mail_aliases

    SELECT * FROM mail_aliases
    INNER JOIN mail_accounts ON mail_aliases.account_id=mail_accounts.id
    WHERE mail_accounts.user_id=@username

mail_attachments

    SELECT * FROM mail_attachments
    WHERE user_id=@username

mail_coll_addresses

    SELECT * FROM mail_coll_addresses
    WHERE user_id=@username

mounts

    SELECT * FROM mounts
    WHERE user_id=@username

notes_meta

    SELECT * FROM notes_meta
    WHERE user_id=@username

notifications

    SELECT * FROM notifications
    WHERE user=@username

notifications_pushtokens

    SELECT * FROM notifications_pushtokens
    WHERE uid=@username

preferences

    SELECT * FROM preferences
    WHERE userid=@username

properties

    SELECT * FROM properties
    WHERE userid=@username

richdocuments_wopi

    SELECT * FROM richdocuments_wopi
    WHERE owner_uid=@username OR editor_uid=@username

schedulingobjects

    SELECT * FROM schedulingobjects
    WHERE principaluri LIKE CONCAT('%', @username, '%')

share

    SELECT * FROM share
    WHERE uid_owner=@username

storages

    SELECT * FROM storages
    WHERE id LIKE CONCAT('%', @username, '%')

talk_participants

    SELECT * FROM talk_participants
    WHERE user_id=@username

termsofservice_sigs

    SELECT * FROM termsofservice_sigs
    WHERE user_id=@username

twofactor_backupcodes

    SELECT * FROM twofactor_backupcodes
    WHERE user_id=@username

twofactor_providers

    SELECT * FROM twofactor_providers
    WHERE uid=@username

twofactor_totp_secrets

    SELECT * FROM twofactor_totp_secrets
    WHERE user_id=@username

users

    SELECT * FROM users
    WHERE uid=@username

vcategory

    SELECT * FROM vcategory
    WHERE uid=@username
