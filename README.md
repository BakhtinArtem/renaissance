

# Renaissance Benchmark Suite

<p align="center">
  <img height="180px" src="https://github.com/D-iii-S/renaissance-benchmarks/raw/master/website/resources/images/mona-lisa-round.png"/>
</p>


The Renaissance Benchmark Suite aggregates common modern JVM workloads,
including, but not limited to, Big Data, machine-learning, and functional programming.
The suite is intended to be used to optimize just-in-time compilers, interpreters, GCs,
and for tools such as profilers, debuggers, or static analyzers, and even different hardware.
It is intended to be an open-source, collaborative project,
in which the community can propose and improve benchmark workloads.

The contents of this README file are automatically generated from the codebase,
and can be refreshed with the `--readme` command-line flag.


### Building the suite

To build the suite and bundle it, you need to first start the included `sbt` build tool:

```
$ tools/sbt/bin/sbt
```

This will open the `sbt` shell, from which you can issue various build commands.
To bundle the suite, run:

```
> renaissanceBundle
```

This will retrieve all the dependencies, compile all the benchmark projects and the harness,
bundle the JARs of the subprojects into the JAR of the suite, copy all the dependencies
to the `target/renaissance` directory, and produce a `tar` file in the `target` directory.


### Running the benchmarks

To run a Renaissance benchmark, you need to have a JRE installed.
This allows you to execute the following `java` command:

```
java -cp '<renaissance-home>/jars/*' org.renaissance.RenaissanceSuite <benchmarks>
```

Above, the `<renaissance-home>` is the path to the root directory of the Renaissance distribution,
and `<benchmarks>` is the list of benchmarks that you wish to run.
For example, you can specify `k-means-scala` as the benchmark.


#### Complete list of command-line options

The following is a complete list of command-line options.

```
Renaissance Benchmark Suite 0.1
Usage: renaissance [options] [benchmark-specification]

  --help                   Prints this usage text.
  -r, --repetitions <value>
                           Number of repetitions of each benchmark
  --policy <value>         Execution policy, one of: fixed
  --plugins <value>        Comma-separated list of class names of plugin implementations.
  --readme                 Regenerates the README file, and does not run anything.
  benchmark-specification  Comma-separated list of benchmarks (or groups) that must be executed.
```


### List of benchmarks

The following is the complete list of benchmarks, separated into groups.


##### apache-spark

- `page-rank` - Runs a number of PageRank iterations, using RDDs. (default repetitions: 20)


##### core

- `dummy` - A dummy benchmark, which does no work. It is used only to test the harness. (default repetitions: 20)


##### jdk-streams

- `scrabble` - Solves the Scrabble puzzle using JDK Streams. (default repetitions: 50)


##### scala-stdlib

- `scala-k-means` - Runs the K-Means algorithm using Scala collections. (default repetitions: 50)


##### twitter-finagle

- `finagle-http` - Sends many small Finagle HTTP requests and awaits the response. (default repetitions: 12)



### Run policies

The suite is designed to support multiple ways of executing a benchmark --
for example, a fixed number of iterations, or a fixed amount of time.
This logic is encapsulated in run policies. Current policies include:

- `fixed` -- Runs the benchmark for a fixed number of iterations.



### Plugins and interfacing with external tools

If you are using an external tool to inspect a benchmark, such as an instrumentation agent,
or a profiler, then you will need to make this tool aware of when a benchmark iteration
is starting and when it is ending.
To allow this, the suite allows specifying custom plugins, which are notified when necessary.
Here is an example of how to implement a plugin:

```
class MyPlugin extends Plugin {
  def onCreation() = {
    // Initialize the plugin after it has been created.
  }
  def beforeIteration(policy: Policy) = {
    // Notify the tool that a benchmark iteration is about to start.
  }
  def afterIteration(policy: Policy) = {
    // Notify the tool that the benchmark iteration has ended.
  }
}
```

Here, the Policy argument describes
the current state of the benchmark.


### Contributing

Please see CONTRIBUTION.md for a description of the contributing process.


### Licensing

The Renaissance Suite comes in two distributions,
and is available under both the MIT license and the GPL3 license.
The GPL distribution with all the benchmarks is licensed under the GPL3 license,
while the MIT distribution includes only those benchmarks that themselves
have less restrictive licenses.

Depending on your needs, you can use either of the two distributions.
The following table contains the licensing information of all the benchmarks:

| Benchmark     | Licenses      | Renaissance Distro |
| ------------- | ------------- |:------------------:|
| page-rank | APACHE2 | MIT |
| scrabble | GPL2 | GPL3 |
| dummy | MIT | MIT |
| scala-k-means | MIT | MIT |
| finagle-http | APACHE2 | MIT |
