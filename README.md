# OpenCVE
OpenCVE, is a platform used to locally import the list of CVEs and perform search &amp; alerting
OpenCVE is a platform used to locally import the list of CVEs and perform searches on it (by vendors, products, CVSS, CWE...). Users subscribe to vendors or products, and OpenCVE alerts them when a new CVE is created or when an update is done in an existing CVE. OpenCVE uses the JSON feed provided by the NVD to update the local list of CVEs. After an initial import, a background task is regularly executed to synchronize the local copy with the NVD feed. If a new CVE is added, or if a change is detected, the subscribers of the related vendors and products are alerted. For now the only method of notification is the mail, but we plan to add other integrations (webhooks, Slack, Jira, PagerDuty, OpsGenie...).

How does it work

It is powered by Postgres running on default port to store all updates and processed with python3 as a fronted on port 8000 (http).

How to use

1. pull and run the container, and map/bind internal port 8000 with base system.

docker run --name mycve -dp 8000:8000 unknowndevice64/cve_alert

2. Initiate feed update in DB update and creating user account.

docker exec -i mycve sh -c 'opencve upgrade-db && opencve import-data --confirm && opencve create-user ud64 info@ud64.com --password "My@pass123" --admin && opencve webserver -b 0.0.0.0:8000'

Open URL 127.0.0.1:8000 in browser and login with,

Username: ud64 and Password: My@pass123

Feel free to close this terminal after getting similar message,

...

[2021-02-23 15:38:10 +0000] [106] [INFO] Listening at: http://0.0.0.0:8000 (106)

...

[2021-02-23 15:38:10 +0000] [112] [INFO] Booting worker with pid: xxx

3. Restart the container/Service.

docker start mycve && docker exec -it mycve bash init
