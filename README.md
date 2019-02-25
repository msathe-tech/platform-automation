# platform-automation-example
Example Repository for using platform-automation

#Prerequisites

1. Refer to the sample credentials file used in this lab in 'credentials' folder.
2. Make sure you crate a private repo and store these credentials files there in the root folder of the repo.
3. Make sure you have git ssh configured to access your private repo.
 
# Stage and configure Rabbit MQ tile

Refer to SAMPLE-creds-toolsmiths-pas-p-rabbitmq.yml

fly -t w sp -p stage-config-rabbit-resourcevars-fix \
--config toolsmiths-pas/pipeline-stage-config-parameterized-localvars-rabbit-fix.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-p-rabbitmq.yml \
--var "product-version=1.15.4" --var "pivnet-product-slug=p-rabbitmq" \
--var "git_private_key=$(cat ~/.ssh/id_rsa)"

# Stage and configure MySQL tile

Refer to SAMPLE-creds-toolsmiths-pas-pivotal-mysql.yml

fly -t w sp -p stage-config-mysql \
--config toolsmiths-pas/pipeline-stage-config-parameterized-localvars.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-pivotal-mysql.yml \
--var "product-version=2.5.3-build.7" --var "pivnet-product-slug=pivotal-mysql" \
--var "feature-ops-file=backups_selector-s3" \
--var "git_private_key=$(cat ~/.ssh/id_rsa)"

# Stage and configure SCS tile

Refer to SAMPLE-creds-toolsmiths-pas-p-spring-cloud-services.yml

fly -t w sp -p stage-config-SCS-defaultvars-fix \
--config toolsmiths-pas/pipeline-stage-config-parameterized-localvars-scs-fix.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-p-spring-cloud-services.yml \
--var "product-version=2.0.6" --var "pivnet-product-slug=p-spring-cloud-services" \
--var "git_private_key=$(cat ~/.ssh/id_rsa)"

# Apply changes

Refer to SAMPLE-creds-toolsmiths-.yml

fly -t w sp -p apply-only \
--config toolsmiths-pas/pipeline-applyonly-parameterized-localvars.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-.yml \
--var "git_private_key=$(cat ~/.ssh/id_rsa)" --var "pivnet-product-slug="
