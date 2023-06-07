# Ants (Spring Challenge 2023)

Source code for CodinGame's Spring Challenge 2023 event.

https://www.codingame.com/contests/spring-challenge-2023

https://www.codingame.com/multiplayer/bot-programming/spring-challenge-2023-ants


# How to use the CLI Referee

WARNING! In order to work, these commands need a JDK 1.8 installed. See build instructions on how to set it up (on Linux).

You can check the JDK is correctly set up by running:
```shell
$ java -version
openjdk version "1.8.0_372"
OpenJDK Runtime Environment (Temurin)(build 1.8.0_372-b07)
OpenJDK 64-Bit Server VM (Temurin)(build 25.372-b07, mixed mode)
```

## Play a match between 2 bots

```shell
$ java  -jar /PATH/TO/spring-2023-ants-cli-referee-1.0-SNAPSHOT.jar -p1name "BOT1" -p1 "bot1.exe" -p2name "BOT2" -p2 "bot2.exe"
387
327
seed=1686036124564
```
Output explanation:
* line 1: player 1 score
* line 2: player 3 score
* line 3: board's seed

Options `-p1name` and `-p2name` are not mandatory. You can use then to give a friendly name to your bots. By default, the bots are named according to the value of options `-p1` and `-p2`.

To use bots written in Python for exemple, replace `-p1 "bot1.exe"` by `-p1 "python3 bot1.py"`

## Play a match between 2 bots on a specific league

```shell
$ java  -jar /PATH/TO/spring-2023-ants-cli-referee-1.0-SNAPSHOT.jar -p1name "BOT1" -p1 "bot1.exe" -p2name "BOT2" -p2 "bot2.exe" -lvl 4
387
327
seed=1686036124564
```
Leagues:
* 1 = Wood 2 : Single base, no eggs, no attacking
* 2 = Wood 1 : Eggs appears
* 3 = Bronze : Multiple bases, attacks enabled
* 4 = Silver/Gold/Legend : Score in inputs

## Play a match between 2 bots on a specific board

```shell
$ java  -jar /PATH/TO/spring-2023-ants-cli-referee-1.0-SNAPSHOT.jar -p1name "BOT1" -p1 "bot1.exe" -p2name "BOT2" -p2 "bot2.exe" -d seed=1686036124564
387
327
seed=1686036124564
```

## Play a match between 2 bots and log game details

```shell
$ java  -jar /PATH/TO/spring-2023-ants-cli-referee-1.0-SNAPSHOT.jar -p1name "BOT1" -p1 "bot1.exe" -p2name "BOT2" -p2 "bot2.exe" -l game.json
158
142
seed=1686065027701
```

## Play a match between 2 bots and view it in your browser locally

```shell
$ java  -jar /PATH/TO/spring-2023-ants-cli-referee-1.0-SNAPSHOT.jar -p1name "BOT1" -p1 "bot1.exe" -p2name "BOT2" -p2 "bot2.exe" -s
http://localhost:8888/test.html
Exposed web server dir: /tmp/codingame
```
Open `http://localhost:8888/test.html` in your browser and enjoy !

## Replay locally in your browser a game from its game details

```shell
$ java  -jar /PATH/TO/spring-2023-ants-cli-referee-1.0-SNAPSHOT.jar -r game.json
http://localhost:8888/test.html
Exposed web server dir: /tmp/codingame
```
Open `http://localhost:8888/test.html` in your browser and enjoy !

## Use with CG Brutal Tester

CG Brutal Tester is here: https://github.com/dreignier/cg-brutaltester

Build it from source code or get a release.

Then you can run it:

```shell
java -jar /PATH/TO/cg-brutaltester-1.0.0.jar -r "java -jar /PATH/TO/spring-2023-ants-cli-referee-1.0-SNAPSHOT.jar" -p1 "bot1.exe" -p2 "bot2.exe" -t 4 -n 100
```

See CG Brutal Tester's README on how to interpret its output and what each option means.

# How to build the CLI Referee

Instructions for Linux (and Windows Subsystem for Linux).

For Windows, you only need to adapt the first part (setting up JDK 1.8 and maven).

```shell
WORK_DIR="/PATH/TO/THE/DIR/WHERE/THIS/REPO/WILL/BE/CLONED"

#
# Setup JDK 1.8
#

JAVA_DIR="/PATH/TO/THE/DIR/WHERE/JDK1.8/WILL/BE/DECOMPRESSED"
# Found on this page: https://wiki.openjdk.org/display/jdk8u/Main
JAVA_URL="https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u392-b08/OpenJDK8U-jdk_x64_linux_hotspot_8u392b08.tar.gz"
cd "$JAVA_DIR"
# Download and decrompress the JDK 1.8 from https://wiki.openjdk.org/display/jdk8u/Main
wget -qO- "$JAVA_URL" | tar xz -C "${JAVA_DIR}"
ln -sf "${JAVA_DIR}/$(basename $(dirname $JAVA_URL))" JDK_1.8
export JAVA_HOME="${JAVA_DIR}/JDK_1.8"
export PATH=$JAVA_HOME/bin:$PATH

# Install maven package from your distribution
# Install NodeJS and NPM from your distribution

#
# Check everything is OK
#

java -version
# Should output something like this:
# openjdk version "1.8.0_372"
# OpenJDK Runtime Environment (Temurin)(build 1.8.0_372-b07)
# OpenJDK 64-Bit Server VM (Temurin)(build 25.372-b07, mixed mode)

mvn -v
# Should output something like this:
# Apache Maven 3.8.7
# Maven home: /usr/share/maven
# Java version: 1.8.0_372, vendor: Temurin, runtime: /PATH/TO/DIR/WHERE/JDK1.8/WILL/BE/DECOMPRESSED/OpenJDK8U-jdk_x64_linux_hotspot_8u372b07/jdk8u372-b07/jre
# Default locale: fr_FR, platform encoding: UTF-8
# OS name: "linux", version: "6.1.0-9-amd64", arch: "amd64", family: "unix"

#
# Get source code
#

git clone https://github.com/FredericLespez/SpringChallenge2023-CliReferee.git "$WORK_DIR"

#
# Build
#

cd "$WORK_DIR"
mvn  clean
rm -f dependency-reduced-pom.xml
pushd src/main/resources/view
rm package-lock.json
npm install
npm start
rm -fr node_modules
popd

#
# Build jar
#

mvn package

# If everything goes well, the jar to launch is in the 'target' directory
ls target/spring-2023-ants-cli-referee-1.0-SNAPSHOT.jar
```
