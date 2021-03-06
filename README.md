# Exhaust Proof-of-Concept

The exhaust proof-of-concept (PoC) seeks to show a simpler implementation model for  autonomous control over physical and virtual computer systems.  Since this is a fairly ambitious goal, the PoC is being implemented in phases.

Autonomous control is based in part on [control theory](https://en.wikipedia.org/wiki/Control_theory).  The simplistic control loop below is a good conceptual scaffold to start framing autonomous control with.

![control loop](https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/Feedback_loop_with_descriptions.svg/400px-Feedback_loop_with_descriptions.svg.png)

The actual autonomous control system _controller_ and _control loop_ will end up being somewhat different and more complex than what might be described as a classic [control system](https://en.wikipedia.org/wiki/Control_system).

## Why Exhaust?

**Data exhaust** is operational byproduct data.  Servers normally generate operational data distinct from application oriented persistent data.  Examples are application and system logs, performance data and accounting data.

>> Common defintions for data exhaust involve data generated on servers as a byproduct of end-user web behaviors.  We have created a more expansive definition to accomodate the PoC.

Emitters collect exhaust data on instrumented systems and ship exhaust data to a central collection point.  Telemetry data is delivered to analytics systems via a message queue.  Analytics systems provide facts and predictions used by activators that determine actions to take.  Actions are implemented by orchestrators that access services provided by plugins to affect changes through sytem agents. 

One PoC goal is to demonstrate the collection of **all exhaust data** from Linux systems and to reliably deliver exhaust data to a central collection point.  In particular, my current favorite system, _Ubuntu 14.04 LTS (server and desktop)_ running on `armv6l` (Raspberry Pi), `armv7l` (Raspberry Pi 2) and `x86_64` (commodoty hardware).

Another PoC goal is to demonstrate a control loop that is able to react to changes in the monitored system and take approprite action to change the system state.

## Control Loop Components

Components within the overall control loop include
- System

  The system under control.  The system contains two required subsystems to support (interfaces with adapters) the controller
  - Agent
  
    An agent enables the controller to affect state changes on the system.  These run time changes can restart services, change configurations, etc.
  - Emitter
  
    Emitters provide operational data indicating state and state changes within the system.
- Controll Function

  The controller function measures the output (provided by emitters embedded in the system) and when appropriate, makes changes in the system (through agents embedded in the system) intended to move the system to the desired state.
  - Collector
  
    Collectors represent telemetry and provide the receiving point for data generated by system embedded emitters.
  - Analyzer
  
    Analyzers take streams of data as input and generate facts and predictions about the state of the system.
  - Activator
  
    Activators take facts and predictions from the Analyzer and determine what action, if any to take.  Actions are identified as named known possible work flows available in the orchestrator.
  - Orchestrator
  
    Orchestrator will implement work flows identified by name.  These workflows are a tasks that will be worked sequentially or in parallel.  Tasks may cause system change directly (through a system agent) or indirectly by changing the systems environment through another (possibly unrelated) system.
  - Adapter
  
    An adapter provides the linkage between tasks and agents.  That is, an adapter implements tasks either directly through agent interfaces on systems or indirectly through interfaces on other systems.

## Controller Collectors

Collectors provide telemetry services.  Currently, the PoC uses RabbitMQ to provide telemetry services.  The repository containing Exhaust Control Function Collector artifacts is [Exhaust-Collector](https://github.com/ThatLarryPearson/exhaust-collector).
    
## System Emitters

Emitters push data into the collector.  That is, the emitters publish messages to a RabbitMQ message queue where Analyzers can subscribe to them.

The current emitters and the associated repositories are:
- [syslog](https://github.com/ThatLarryPearson/exhaust-emitter-syslog)
- [CLI Logging](https://github.com/ThatLarryPearson/exhaust-emitter-cli-logging)
- [Process Accounting](https://github.com/ThatLarryPearson/exhaust-emitter-process-accounting)
- [Performance Metrics](https://github.com/ThatLarryPearson/exhaust-emitter-performance-metrics)
- Network Monitoring

## Tools

We all stand on the shoulders of giants.  My work wouldn't be possible without the gracious contributions made by teams providing the following tools (this list is woefully inadequate):
- Python IDE [PyCharm Community Edition](https://www.jetbrains.com/pycharm)

## Licensing

The MIT License (MIT)

Copyright &copy; 2015 Lawrence Bennett Pearson

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


