# Usage

This document assumes you have followed the [setup](setup.md) procedures and
have a running Jenkins instance using Docker.

## Feature development

1.  Create a feature branch called `feature/1` from `master` branch

    ```bash
    (master): git checkout -b feature/1
    ```

2.  Open any file, e.g. README.md, and make a change

3.  Deploy and test your changes

    ```bash
    cd currency-v1
    mvn install -Ptest -Denv.APIGEE_ORG={ORG} -Denv.APIGEE_USERNAME={username} -Denv.APIGEE_PASSWORD={password} -Denv.API_DOMAIN_TEST={apigee_proxy_domain}
    ```

    This command will deploy the bundle to `test` environment as
    `currency-<yourname>v1`. Description of the proxy will be set to `commit
    from a local branch by <your-name>`.

4.  Commit your changes to feature branch

    ```bash
    git commit -am "changed README.md"
    ```

5.  Push feature branch to remote. You will see at this point that Jenkins will
    automatically start `currency-v1-features` job which will deploy the bundle
    as `currency-jenkinsv1` to `test` environment. Description of the proxy will
    be set to `commit <commit_hash> from feature/1 branch by jenkins`.

6.  Create a pull request - note that even thought this is not a must-have but
    it is good to demonstrate a best-practice team development scenario where
    merge/pull requests are used for review/discussion of features before they
    are merged to master branch.

## Merge to master

Merge `feature/1` branch to `master` after review. You can either use pull/merge
request user interfaces or from shell:

```bash
git checkout master
git merge --no-ff feature/1
git push
git branch -d feature/1
```

You will see at this point that Jenkins will automatically start
`currency-v1-master` job which will deploy the bundle as `currency-v1` to `test`
environment.

Description of the proxy will be set to `commit <commit_hash> from
master branch by jenkins`.

## Continuous Delivery

Once `currency-v1-master` job is completed successfully, it will automatically
merge `master` to `prod` branch which will trigger `currency-v1-prod` Jenkins
job.

Production job will deploy the bundle as `currency-v1` on `prod`
environment.

Description of the proxy will be set to `commit <commit_hash> from
master branch by jenkins`.
