# Flume 1.7.0 Taildir Source

Flume 1.7.0 implements Taildir Source in [FLUME-2498].


## Description of Taildir Source

This is the proposal of implementing a new tailing source.

This source watches the specified files, and tails them in nearly real-time once appends are detected to these files.

1. This source is reliable and will not miss data even when the tailing files rotate.
2. It periodically writes the last read position of each file in a position file using the JSON format.
3. If Flume is stopped or down for some reason, it can restart tailing from the position written on the existing position file.
4. It can add event headers to each tailing file group.

A attached patch includes a config documentation of this.
This source requires Unix-style file system and Java 1.7 or later.

## Add feature

Flume-NG's Taildir Source does not support recursivly traversal the directory. So I have developed this feature to support monitor sub-directories recursivly.

**NOTE 1: This plugin is built for Flume-NG 1.6.0 and will not work on Flume-OG. It's worth noting that if you use it on Flume-NG 1.6.0, you should also make some change in other modules because of the difference between 1.6.0 and 1.7.0, the two major versions. For instance, flume-ng-core.  Of course, you can transfer this feature to Flume-NG 1.7.0 easily without any changes.**

**NOTE 2: It lacks comprehensive test coverage. Of course contributions are welcome to make its more stable and useful.**

## Compilation

The project is maintained by [Maven](http://maven.apache.org/).

## Installation instructions

After your compilation, you should ship the target jar `flume-taildir-source-1.6.0.jar` to the `$FLUME_HOME/flume-ng/lib/`. Then you can edit flume.conf to use the Taildir Source.

Now follows a brief overview of Taildir Source with usage instructions.

## Sources

### Taildir Source

The 'Taildir Source' is extended from original flume 1.7.0's Taildir Source, so you can use other properties just like using Taildir Source.

Example config:

```
agent.sources.tail_dir_source.type = com.urey.flume.source.taildir.TaildirSource
agent.sources.tail_dir_source.channels = memory_channel
agent.sources.tail_dir_source.positionFile = /tmp/taildir_position.json
agent.sources.tail_dir_source.filegroups = f1
agent.sources.tail_dir_source.filegroups.f1 = /home/urey/yarn_log/.*
agent.sources.tail_dir_source.batchSize = 100
agent.sources.tail_dir_source.backoffSleepIncrement  = 1000
agent.sources.tail_dir_source.maxBackoffSleep  = 5000
agent.sources.tail_dir_source.recursiveDirectorySearch = true
# agent.sources.tail_dir_source.fileHeader = true
# agent.sources.tail_dir_source.yarnApplicationHeader = true
# agent.sources.tail_dir_source.yarnContainerHeader = true
```

The last three configurations are customized for collecting yarn nodemanger's log to add headers. Of course, you can ignore and remove them.










