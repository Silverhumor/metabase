## Copying contents of one Metabase instance to another

Metabase's serialization feature allows you to create a snapshot (called a dump) of the contents of a Metabase instance that can then be loaded into another instance.

This lets you do things like create a set of dashboards in one Metabase instance and then easily copy those dashboards to a number of other Metabase instances that you've set up for your customers. You could also use this feature to enable a staging-to-production workflow for important dashboards or reports by dumping from a staging instance of Metabase and then loading that dump into your production instance(s).

### What gets dumped and loaded
Currently a dump file consists of the following Metabase artifacts:
* Collections
* Dashboards
* Saved questions
* Pulses
* Segments and Metrics defined in the Data Model
* Archived collections, dashboards, saved questions, or pulses

It also contains a number of system settings:
* Admin Panel settings, except for permissions
* Database connection settings
* Data Model settings

The dump file does _not_ contain:
* Permission settings
* User accounts or settings
* Alerts on saved questions

### Before creating or loading a dump
If your instance is currently running, you will need to stop it first before creating or loading a dump, unless your Metabase application database supports concurrent reads. The default application database type, H2, does not support concurrent reads.

### Creating a data dump
To create a dump of a Metabase instance, use the following command in your terminal:

`java -jar metabase.jar dump [dump_name] --user [example@example.com]`

The optional `--user` flag is used to specify a default administrator account for cases when this dump file is loaded into a blank Metabase instance. This user will also be marked as the creator of all artifacts that are copied over to the instance. If this flag isn't specified, it's assumed that the instance into which you're loading already has an admin user.

### Loading a dump file
To load a dump file into another instance of Metabase, use the following command, where `[my_dump]` is the path to the dump file you want to load:

`java -jar metabase.jar load [my_dump] --mode [skip/update] --on-error [continue/abort]`

The `--mode` flag lets you specify what to do when encountering a duplicate dashboard, question, etc. It can either `skip` that item and do nothing to it, or `update` it with the version being loaded. The default is `skip`.

The `--on-error` flag allows you to specify whether the load process should keep going or stop when there's an error. The default is `continue`.

Both of these flags are optional.

---

## That's it!
Still need help? Feel free to reach out to us at the support email address you were provided.