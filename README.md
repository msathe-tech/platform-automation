# platform-automation-example
Example Repository for using platform-automation

#Quick help

Make sure you have slug specific creds file in your private repo.


# Setup RabbitMQ tile
Fully parameterized stage & config - rabbit. resource-vars fix

fly -t w sp -p stage-config-rabbit-resourcevars-fix \
--config toolsmiths-pas/pipeline-stage-config-parameterized-localvars-rabbit-fix.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-p-rabbitmq.yml \
--var "product-version=1.15.4" --var "pivnet-product-slug=p-rabbitmq" \
--var "feature-ops-file=syslog_selector-disabled" \
--var "git_private_key=$(cat ~/.ssh/id_rsa)"

# Setup MySQL tile

Fully parameterized (no slug specifics in pipeline yml) stage & config - mysql

fly -t w sp -p stage-config-mysql \
--config toolsmiths-pas/pipeline-stage-config-parameterized-localvars.yml \
--load-vars-from ~/workspace/pcf-automation/platform-automation-private/creds-toolsmiths-pas-pivotal-mysql.yml \
--var "product-version=2.5.3-build.7" --var "pivnet-product-slug=pivotal-mysql" \
--var "feature-ops-file=backups_selector-s3" \
--var "git_private_key=$(cat ~/.ssh/id_rsa)"
