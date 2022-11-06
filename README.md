# Showcase for building react libraries with vite

## How to run it locally

### Running local repo using Verdaccio in docker
Prepare folder structure

    ./verdaccio
    ├── conf
    │   └── config.yaml
    ├── plugins
    └── storage

With config.yaml:

    storage: /verdaccio/storage
    plugins: /verdaccio/plugins
    web:
        title: Verdaccio
    auth:
        htpasswd:
            file: ./htpasswd
    uplinks:
        npmjs:
            url: https://registry.npmjs.org/                                
    packages:
        '@*/*':
            access: $all
            publish: $authenticated
            unpublish: $authenticated
            proxy: npmjs
        '**':
            access: $all
            publish: $authenticated
            unpublish: $authenticated
            proxy: npmjs
    middlewares:
        audit:
            enabled: true
    logs:
        - {type: stdout, format: pretty, level: http}

and run it on docker

    docker run -it --detach \
        --publish 4873:4873 \
        --volume `pwd`/conf:/verdaccio/conf \
        --volume `pwd`/storage:/verdaccio/storage \
        --volume `pwd`/plugins:/verdaccio/plugins \
        --name verdaccio \
        verdaccio/verdaccio

### Adding new user
    npm adduser --registry http://localhost:4873

### Build library
    cd vrbt-lib
    yarn install
    yarn build

### Publish library
    npm publish --registry http://localhost:4873

### Run test project
    cd test-site
    yarn install --registry http://localhost:4873
    yarn run dev