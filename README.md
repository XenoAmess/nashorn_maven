- What is this repo?

I managed to push nashorn to maven central...

Of course please use it following GPL2-WITH-CLASSPATH-EXCEPTION,
just like when you use other jdk lib.

maven:

```
<dependency>
  <groupId>com.xenoamess</groupId>
  <artifactId>nashorn</artifactId>
  <version>jdk8u265-b01</version>
</dependency>
```

gradle:

```
implementation 'com.xenoamess:nashorn:jdk8u265-b01'
```

----------
----------
----------

- What is Nashorn?

Nashorn is a runtime environment for programs written in ECMAScript 5.1
that runs on top of JVM.

- How to find out more about ECMAScript 5.1?

The specification can be found at

    http://www.ecma-international.org/publications/standards/Ecma-262.htm

- How to checkout sources of Nashorn project?

Nashorn project uses Mercurial source code control system. You can
download Mercurial from http://mercurial.selenic.com/wiki/Download

Information about the forest extension can be found at

    http://mercurial.selenic.com/wiki/ForestExtension

and downlaoded using

    hg clone https://bitbucket.org/gxti/hgforest

You can clone Nashorn Mercurial forest using this command:

    hg fclone http://hg.openjdk.java.net/nashorn/jdk8 nashorn~jdk8
    
To update your copy of the forest (fwith the latest code:

    (cd nashorn~jdk8 ; hg fpull)
    
Or just the nashorn subdirectory with

    (cd nashorn~jdk8/nashorn ; hg pull -u)
    
To learn about Mercurial in detail, please visit http://hgbook.red-bean.com.

- How to build?

To build Nashorn, you need to install JDK 8. You may use the Nashorn
forest build (recommended) or down load from java.net.  You will need to
set JAVA_HOME environmental variable to point to your JDK installation
directory.

    cd nashorn~jdk8/nashorn/make
    ant clean; ant

- How to run?

Use the jjs script (see RELESE_README):

    cd nashorn~jdk8/nashorn
    sh bin/jjs <your .js file>

Nashorn supports javax.script API. It is possible to drop nashorn.jar in
class path and request for "nashorn" script engine from
javax.script.ScriptEngineManager. 

Look for samples under the directory test/src/jdk/nashorn/api/scripting/.

- Documentation

Comprehensive development documentation is found in the Nashorn JavaDoc. You can
build it using:

    cd nashorn~jdk8/nashorn/make
    ant javadoc
    
after which you can view the generated documentation at dist/javadoc/index.html.

- Running tests

Nashorn tests are TestNG based. Running tests requires downloading the
TestNG library and placing its jar file into the test/lib subdirectory. This is
done automatically when executing the "ant externals" command to get external
test suites (see below).

Once TestNG is properly installed, you can run the tests using:
    cd make
    ant clean test
    
You can also run the ECMA-262 test suite with Nashorn. In order to do
that, you will need to get a copy of it and put it in
test/script/external/test262 directory. A convenient way to do it is:

   git clone https://github.com/tc39/test262 test/script/external/test262
    
Alternatively, you can check it out elsewhere and make
test/script/external/test262 a symbolic link to that directory. After
you've done this, you can run the ECMA-262 tests using:

    cd nashorn~jdk8/nashorn/make
    ant test262

Ant target to get/update external test suites:

    ant externals
    ant update-externals
    
These tests take time, so we have a parallelized runner for them that
takes advantage of all processor cores on the computer:

    cd nashorn~jdk8/nashorn/make
    ant test262parallel
    
- How to write your own test?

Nashorn uses it's own simple test framework. Any .js file dropped under
nashorn/test directory is considered as a test. A test file can
optionally have .js.EXPECTED (foo.js.EXPECTED for foo.js) associated
with it. The .EXPECTED file, if exists, should contain the output
expected from compiling and/or running the test file.

The test runner crawls these directories for .js files and looks for
JTReg-style @foo comments to identify tests.

    * @test - A test is tagged with @test.

    * @test/fail - Tests that are supposed to fail (compiling, see @run/fail
      for runtime) are tagged with @test/fail.

    * @test/compile-error - Test expects compilation to fail, compares
      output.

    * @test/warning - Test expects compiler warnings, compares output.

    * @test/nocompare - Test expects to compile [and/or run?]
      successfully(may be warnings), does not compare output.

    * @subtest - denotes necessary file for a main test file; itself is not
      a test.

    * @run - A test that should be run is also tagged with @run (otherwise
      the test runner only compiles the test).

    * @run/fail - A test that should compile but fail with a runtime error.

    * @run/ignore-std-error - script may produce output on stderr, ignore
      this output.

    * @argument - pass an argument to script.

    * @option \ - pass option to engine, sample.

/**
 * @option --dump-ir-graph
 * @test
 */
