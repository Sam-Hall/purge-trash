## Purge Trash - Confluence User Macro
Tested on Confluence versions 5.7.5 - 5.10.4

This is a workaround to the lack of a Purge All feature within Confluence. For background, see https://jira.atlassian.com/browse/CONF-18160

### Installation
Install it into your instance by creating a new User Macro and fill in the blanks. \<CONFLUENCE_URL>/admin/usermacros.action

### Usage
The macro is pretty self explanatory. Put it on a page and view the page. Take care not to refresh the page multiple times, as multiple invocations in parallel can lead to errors. Also note that the Macro Browser preview pane will also trigger an invocation of the script.

There are some options to limit the amount of damage it can do. By default, the script stops after 100 trash items are processed. This is just to prevent the page taking too long to load, consuming server resources for too long and possibly triggering errors due to timeouts. Increase/decrease this limit depending on available server resources, server performance and the estimated sizes of the pages being deleted (large pages require more time/resources).

The script wont perform any purge operations until the "simulate" option is unchecked as a safety mechanism while configuration settings are tweaked.

This script only solves part of the problem described in CONF-18160. To schedule this to run periodically, a cronjob could be setup to call the page with Confluence Administrator credentials via curl or wget.

### Resolving Duplicate Attachments
The page will need to be resurrected to fix this problem. Attempting to purge a page with duplicate attachments is not possible. Confluence doesn't usually permit two attachments with the same name to be added to a page, but some times it does happen due to some illusive bug in the upload process. For background, see  https://jira.atlassian.com/browse/CONF-7882

The direct link to successfully delete one copy of the attachment looks like this. 
\<CONFLUENCE_URL>/pages/confirmattachmentremoval.action?pageId=\<ID>&fileName=\<FILENAME>
While it's of little consequence if you're about to purge it anyway, it's worth noting that it's unclear which copy will be removed. Confluence will only remove one copy at a time, so if you have several duplicates of the same file you'll have to keep calling that link until only one remains.