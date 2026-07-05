# Getting your enterprise started with the GitHub Support portal

Learn how to start using the GitHub Support portal for issues related to your enterprise.

## About the GitHub Support portal for enterprises

You can use the [GitHub Support portal](https://support.github.com/) to create and manage support tickets about your GitHub Enterprise Server instance.

To benefit from Premium Support SLAs and ticket collaboration features, you must associate your ticket with your GitHub Enterprise Server instance in one of two ways.

1. If your institution has an enterprise account on GitHub.com, you have a user account on GitHub.com, and that user account has been granted support entitlements for the enterprise account, you can select the enterprise account when creating a ticket. For more information about enterprise accounts, see [About enterprise accounts](/en/enterprise-server@3.21/admin/overview/about-enterprise-accounts).

   * The majority of GitHub Enterprise Server customers already have an enterprise account on GitHub.com. If you're not sure whether you do, first check with your team.
   * If your team confirms that you do not have an enterprise account on GitHub.com, you can [submit a request](https://support.github.com/contact?comments=%3E+Please+provide+the+following+information+and+someone+will+be+in+touch+to+help+you+setup+your+enterprise+account.%0A%0A%23%23%23%23+Company+name+%28required%29%0A%0A%23%23%23%23+Email+address+or+GitHub+login+of+the+person+who+should+be+the+initial+owner+of+the+enterprise+account+%28required%29%0A%0A%23%23%23%23+Is+there+anything+else+we+should+know+to+help+us+identify+your+account%3F+%28optional%29%0A%3E+Attaching+a+GitHub+Enterprise+Server+diagnostics+file+here+can+help+us+identify+your+account+by+license+reference+number%0A%0A\&subject=Enterprise+Account+Request\&tags=new-ea) for a new one.
   * Then, for the best experience, follow the steps below before using the [GitHub Support portal](https://support.github.com/) to create tickets about the enterprise account.

2. If you're sure you do not have an enterprise account on GitHub.com, you have not been configured as a support-entitled member by an enterprise owner, or you cannot sign in with to your GitHub.com account, you can provide a copy of your license key or diagnostics file by using the [Get help with GitHub Enterprise Server](https://support.github.com/contact/enterprise-by-license) form.

## Getting started with the GitHub Support portal

Before you start creating tickets associated with your enterprise account on GitHub.com, we recommend completing the following steps.

1. Identify the user on GitHub.com who is an owner of your enterprise account.
2. Configure a verified domain. For more information, see [Verifying or approving a domain for your enterprise](/en/enterprise-server@3.21/admin/configuration/configuring-your-enterprise/verifying-or-approving-a-domain-for-your-enterprise).
3. Add owners, billing managers, or support-entitled members to your enterprise account. For more information, see [Inviting people to manage your enterprise](/en/enterprise-cloud@latest/admin/user-management/managing-users-in-your-enterprise/inviting-people-to-manage-your-enterprise) and [Managing support entitlements for your enterprise](/en/enterprise-cloud@latest/admin/user-management/managing-users-in-your-enterprise/managing-support-entitlements-for-your-enterprise).# docker-compose-influxdb-grafana

Multi-container Docker app built from the following services:

* [InfluxDB](https://github.com/influxdata/influxdb) - time series database
* [Chronograf](https://github.com/influxdata/chronograf) - admin UI for InfluxDB
* [Grafana](https://github.com/grafana/grafana) - visualization UI for InfluxDB

Useful for quickly setting up a monitoring stack for performance testing. Combine with [serverless-artillery](https://github.com/Nordstrom/serverless-artillery) and [artillery-plugin-influxdb](https://github.com/Nordstrom/artillery-plugin-influxdb) to create a performance testing environment in minutes.

Now with built-in support for influxDB 2.x 

## Quick Start

To start the app:

1. Install [docker-compose](https://docs.docker.com/compose/install/) on the docker host.
1. Clone this repo on the docker host.
1. Optionally, change default credentials or Grafana provisioning.
1. Run the following command from the root of the cloned repo:
```
docker-compose up -d
```

To stop the app:

1. Run the following command from the root of the cloned repo:
```
docker-compose down
```

## Ports

The services in the app run on the following ports:

| Host Port | Service |
| - | - |
| 3000 | Grafana |
| 8086 | InfluxDB |
| 127.0.0.1:8888 | Chronograf |

Note that Chronograf does not support username/password authentication. Anyone who can connect to the service has full admin access. Consequently, the service is not publically exposed and can only be access via the loopback interface on the same machine that runs docker.

If docker is running on a remote machine that supports SSH, use the following command to setup an SSH tunnel to securely access Chronograf by forwarding port 8888 on the remote machine to port 8888 on the local machine:

```
ssh [options] <user>@<docker-host> -L 8888:localhost:8888 -N
```

## Volumes

The app creates the following named volumes (one for each service) so data is not lost when the app is stopped:

* influxdb-storage
* chronograf-storage
* grafana-storage

## Users

The app creates two admin users - one for InfluxDB and one for Grafana. By default, the username and password of both accounts is `admin`. To override the default credentials, set the following environment variables before starting the app:

* `INFLUXDB_USERNAME`
* `INFLUXDB_PASSWORD`
* `GRAFANA_USERNAME`
* `GRAFANA_PASSWORD`

## Database

The app creates a default InfluxDB database called `db0`.

## Data Sources

The app creates two Grafana data sources called `InfluxDB(GraphQL)` (with legacy authentication) and `InfluxDB(Flux)` which are connected to the default IndfluxDB database.

To provision additional data sources, see the Grafana [documentation](http://docs.grafana.org/administration/provisioning/#datasources) and add a config file to `./grafana-provisioning/datasources/` before starting the app.

## InfluxDB scripts

Scripts located in `influxDB-scripts` are excuted after the inital setup task of influxDB. At the moment, the script `setup-v1.sh` takes care of the `dbrp` mappings and the creation of v1 compatible users.

## Dashboards

By default, the app does not create any Grafana dashboards. An example dashboard that's configured to work with [artillery-plugin-influxdb](https://github.com/Nordstrom/artillery-plugin-influxdb) is located at `./grafana-provisioning/dashboards/artillery.json.example`. To use this dashboard, rename it to `artillery.json`.

To provision additional dashboards, see the Grafana [documentation](http://docs.grafana.org/administration/provisioning/#dashboards) and add a config file to `./grafana-provisioning/dashboards/` before starting the app.
