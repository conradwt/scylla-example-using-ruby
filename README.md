# Scylla Example Using Ruby

The purpose of this step by step tutorial is to provide a very simple example of configuring and using Scylla database engine with the Ruby Language.

## Requirements

- [Docker Desktop for Mac
  ](https://hub.docker.com/editions/community/docker-ce-desktop-mac)

- Node 18.1.0 or newer

- Rails 6.1.6 or newer

- Ruby 3.1.2 or newer

- Yarn 1.22.18 or newer

Note: This tutorial was updated on macOS 12.3.1.

## Communication

- If you **need help**, use [Stack Overflow](http://stackoverflow.com/questions/tagged/scylla). (Tag 'scylla')
- If you'd like to **ask a general question**, use [Stack Overflow](http://stackoverflow.com/questions/tagged/scylla).
- If you **found a bug**, open an issue.
- If you **have a feature request**, open an issue.
- If you **want to contribute**, submit a pull request.

## Installation, Setup, and Usage

1.  Open new terminal window

2.  Start a single node cluster

    ```zsh
    docker-compose up -d
    ```

3.  Check the status of your cluster

    ```zsh
    docker-compose exec some-scylla nodetool status
    ```

    Note: One should see that the node status as Up Normal (UN) that looks similar to the following:

    ```text
    Datacenter: datacenter1
    =======================
    Status=Up/Down
    |/ State=Normal/Leaving/Joining/Moving
    --  Address     Load       Tokens       Owns    Host ID                               Rack
    UN  172.19.0.2  582.5 KB   256          ?       e61cf276-c860-4990-bf03-37161414aed2  rack1

    Note: Non-system keyspaces don't have the same replication settings, effective ownership information is meaningless
    ```

4.  Generate a new Rails application

    ```zsh
    rails new blog --skip-active-record --skip-active-storage -T
    ```

5.  Add the Ruby cequel gem

    ```zsh
    cd blog
    bundle add cequel
    bundle add activemodel-serializers-xml
    ```

6.  Generate scaffold of the application

    ```zsh
    rails g scaffold post title body
    ```

7.  Add the following as the first route within config/routes.rb file:

    ```ruby
    root 'posts#index'
    ```

8.  Create app/models/post.rb file with the following content:

    ```ruby
    class Post
      include Cequel::Record

      key :id, :timeuuid, auto: true
      column :title, :text
      column :body, :text

      timestamps
    end
    ```

9.  Create a default configuration file

    ```zsh
    rails g cequel:configuration
    ```

10. Initialize keyspace (.i.e. database)

    ```zsh
    rails cequel:keyspace:create
    ```

11. Synchronize your Rails model schemas with the keyspace

    ```zsh
    rails cequel:migrate
    ```

12. Start the Rails server

    ```
    $ rails s
    ```

13. Play with the application

    ```zsh
    open http://localhost:3000
    ```

14. Remove the keyspace

    ```zsh
    rails cequel:keyspace:drop
    ```

15. Stop a single node cluster

    ```zsh
    docker-compose down
    ```

16. Cleanup Docker artifacts

    ```zsh
    docker system prune -f -a --volumes
    ```

---

## References

- [Scylla](https://www.scylladb.com/open-source-community/)

- [Cequel](https://github.com/cequel/cequel)

## Support

Bug reports and feature requests can be filed for the scylla-example-using-ruby project here:

- [File Bug Reports and Features](https://github.com/conradwt/scylla-example-using-ruby/issues)

## Contact

Follow Conrad Taylor on Twitter ([@conradwt](https://twitter.com/conradwt))

## Creator

- [Conrad Taylor](http://github.com/conradwt) ([@conradwt](https://twitter.com/conradwt))

## License

This repository is released under the [MIT License](./LICENSE.md).

## Copyright

&copy; Copyright 2020 - 2022 Conrad Taylor. All Rights Reserved.
