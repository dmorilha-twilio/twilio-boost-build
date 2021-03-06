Boost-Build
===========

Scripts to build boost for different platforms (macOS, iOS, tvOS, watchOS, Linux, Android) and package it as a Maven tarball and/or an Apple Framework.

Libraries are packaged in Twilio style, grouped by variant (debug/release) and then by platform architecture beneath the variant. This makes debug/x86, debug/armv7 etc folders that are consumable by Twilio builds.

Unpacking these tarballs into a single directory will create all necessary libraries underneath the abovementioned folders together with a top-level includes/boost directory for headers.

To build packages use ./boost.sh with appropriate argument `-android`, `-ios`, `-osx`, `-linux` or `-linux-cxx11-abi-disabled`.
If not specified, it will attempt to build all. It requires Maven to be installed for deployment - if you do not deploy to Maven-based repositories you can ignore it.

To find the directories from cmake specify two cmake defines, e.g.:

    cmake -DBOOST_INCLUDEDIR=toplevel/include -DBOOST_LIBRARYDIR=toplevel/lib/release/armv7

To put all consumed libraries into single directory so that cmake could find them use this maven
plugin invocation:

    <plugin>
        <artifactId>maven-antrun-plugin</artifactId>
        <executions>
            <execution>
                <phase>initialize</phase>
                <goals>
                    <goal>run</goal>
                </goals>
                <configuration>
                    <tasks>
                        <!-- todo copy beast to boost headers... -->
                        <copy todir="${project.build.directory}/dependency/${build.platform}/boost-headers-${dependencies.boost.version}">
                            <fileset dir="${project.build.directory}/dependency/${build.platform}/beast-headers-${dependencies.websockets.version}"/>
                        </copy>
                        <!-- flatten all libraries to boost/lib so that CMake can find it -->
                        <copy todir="${project.build.directory}/dependency/${build.platform}/boost">
                            <fileset dir="${project.build.directory}/dependency/${build.platform}/boost-system-${dependencies.boost.version}"/>
                        </copy>
                    </tasks>
                </configuration>
            </execution>
        </executions>
    </plugin>


Attributions
============

Build script is based on https://github.com/faithfracture/Apple-Boost-BuildScript


License
=======

Boost Software License - Version 1.0 - August 17th, 2003

Permission is hereby granted, free of charge, to any person or organization
obtaining a copy of the software and accompanying documentation covered by
this license (the "Software") to use, reproduce, display, distribute,
execute, and transmit the Software, and to prepare derivative works of the
Software, and to permit third-parties to whom the Software is furnished to
do so, all subject to the following:

The copyright notices in the Software and this entire statement, including
the above license grant, this restriction and the following disclaimer,
must be included in all copies of the Software, in whole or in part, and
all derivative works of the Software, unless such copies or derivative
works are solely in the form of machine-executable object code generated by
a source language processor.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. IN NO EVENT
SHALL THE COPYRIGHT HOLDERS OR ANYONE DISTRIBUTING THE SOFTWARE BE LIABLE
FOR ANY DAMAGES OR OTHER LIABILITY, WHETHER IN CONTRACT, TORT OR OTHERWISE,
ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
DEALINGS IN THE SOFTWARE.
