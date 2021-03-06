This directory contains some very simple loadtests, written using
the "loads" framework:

    https://github.com/mozilla/loads


To run them, you will need the following dependencies:

  * Python development files (e.g. python-dev or python-devel package)
  * Virtualenv (e.g. python-virtualenv package)
  * ZeroMQ development files (e.g. libzmq-dev package)
  * (for megabench) ssh access to the mozilla loads cluster

You'll also need to configure the server under test to talk to a mock
FxA OAuth verifier.  You can use this preconfigured one, which proxies
valid tokens through to the FxA stage environment:

  https://mock-oauth-stage.dev.lcip.org

Or you can deploy one for your own use using the `mock-oauth-cfn.yml`
CloudFormation template, like this:

  $> aws cloudformation deploy \
        --template-file=mock-oauth-cfn.yml \
        --stack-name my-mock-oauth-stack \
        --capabilities CAPABILITY_IAM \
        --parameter-overrides \
            DomainName=my-mock-oauth.dev.lcip.org

Then do the following:

  $> make build       # installs local environment with all dependencies
  $> make test        # runs a single test, to check that everything's working
  $> make bench       # runs a longer, higher-concurrency test.
  $> make megabench   # runs a really-long, really-high-concurrent test
                      # using https://loads.services.mozilla.com

To hit a specific server you can specify the SERVER_URL make variable, like
this:

  $> make test SERVER_URL=https://token.stage.mozaws.net

