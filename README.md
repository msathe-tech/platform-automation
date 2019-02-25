# platform-automation-example
Example Repository for using platform-automation

#Quick help

Make sure you have a common creds in your private repo in following format.

File name - creds-toolsmiths-pas-.yml

foundation: toolsmiths-pas

opsman_host: pcf.somedomain.cf-app.com

opsman_userid: pivotalcf

opsman_password: opsman-password

opsman_decryption_passphrase:

pivnet_token: Your pivnet token

# Stage and configure Rabbit MQ tile

Prereq -

Following need to be available in creds-toolsmiths-pas-p-rabbitmq.yml of your private repo

1. all params in creds-toolsmiths-pas-.yml

2. Add following to that list
singleton_availability_zone: us-central1-f
network_name: your-pas-subnet
service_network_name: your-services-subnet

on_demand_broker_plan_1_rabbitmq_az_placement: AZ in array format, e.g. [ 'us-central1-f' ]

multitenant_support/enabled/server_admin_credentials_identity: username
multitenant_support/enabled/server_admin_credentials_password: password

multitenant_support/enabled/broker_operator_set_policy_enabled:
multitenant_support/enabled/disk_alarm_threshold:
multitenant_support/enabled/server_cluster_partition_handling: pause_minority
multitenant_support/enabled/server_ports: 15672, 5672, 5671, 1883, 8883, 61613, 61614, 15674
multitenant_support/enabled/server_ssl_verification_depth: 5

fly -t w sp -p stage-config-rabbit-resourcevars-fix \
--config toolsmiths-pas/pipeline-stage-config-parameterized-localvars-rabbit-fix.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-p-rabbitmq.yml \
--var "product-version=1.15.4" --var "pivnet-product-slug=p-rabbitmq" \
--var "git_private_key=$(cat ~/.ssh/id_rsa)"

# Stage and configure MySQL tile

Prereq -

Following need to be available in creds-toolsmiths-pas-pivotal-mysql of your private repo

1. all params in creds-toolsmiths-pas-.yml

2. Add following to that list

deprecated_bindings_string: X
global_recipient_email: some@email.id
network_name: your-pas-subnet

plan1_selector/single_node/az_multi_select: AZ in array format, e.g. [ 'us-central1-f' ]
plan2_selector/single_node/az_multi_select: AZ in array format, e.g. [ 'us-central1-f' ]
plan3_selector/single_node/az_multi_select: AZ in array format, e.g. [ 'us-central1-f' ]

service_network_name: hermosabeach-services-subnet
singleton_availability_zone: us-central1-f

backups_selector/s3/access_key_id: your-aws-access-id
backups_selector/s3/secret_access_key: your-aws-access-key
backups_selector/s3/bucket_name: bucket name in east-1 region
backups_selector/s3/path: some-path e.g. mysql
backups_selector/s3/enable_email_alerts:
backups_selector/s3/cron_schedule: desired cron cron_schedule (e.g. 0 */8 * * *)

**
fly -t w sp -p stage-config-mysql \
--config toolsmiths-pas/pipeline-stage-config-parameterized-localvars.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-pivotal-mysql.yml \
--var "product-version=2.5.3-build.7" --var "pivnet-product-slug=pivotal-mysql" \
--var "feature-ops-file=backups_selector-s3" \
--var "git_private_key=$(cat ~/.ssh/id_rsa)"

# Stage and configure SCS tile
Prereq -

Following need to be available in creds-toolsmiths-pas-p-spring-cloud-services of your private repo

1. all params in creds-toolsmiths-pas-.yml

2. Add following to that list

singleton_availability_zone: us-central1-f
network_name: your-pas-subnet
service_network_name: your-services-subnet

fly -t w sp -p stage-config-SCS-defaultvars-fix \
--config toolsmiths-pas/pipeline-stage-config-parameterized-localvars-scs-fix.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-p-spring-cloud-services.yml \
--var "product-version=2.0.6" --var "pivnet-product-slug=p-spring-cloud-services" \
--var "git_private_key=$(cat ~/.ssh/id_rsa)"

# Apply changes

fly -t w sp -p apply-only \
--config toolsmiths-pas/pipeline-applyonly-parameterized-localvars.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-.yml \
--var "git_private_key=$(cat ~/.ssh/id_rsa)" --var "pivnet-product-slug="
