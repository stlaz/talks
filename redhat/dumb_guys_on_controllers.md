%title: Dumb guy's guide to kubernetes controllers
%author: Stanislav Láznička

-> ▛▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▜ <-

-> # Dumb Guy's Guide to Kubernetes Controllers <-
-> (based on library-go) <-

-> ▙▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▟ <-


---

-> # Kubernetes controllers <-

---

## Controller
* runs endlessly
* regulates a certain state of the system
  - -> usually operates on a specific type of a resource
* reports its status (e.g. number of updated pods of a *Deployment*)

ref: https://kubernetes.io/docs/concepts/architecture/controller/

---

## A typical kubernetes controller consists of:
* a method defining what needs to be done
    - usually contains _*sync*_ in its name
* a work queue
* a set of informers filling the queue with events
    - events are sometimes filtered
    - events get turned into queue keys by _key functions_
* a set of kube clients to communicate with the API

---

## * Homework 1 *

Study the [ServiceAccount Tokens Controller](https://github.com/kubernetes/kubernetes/blob/master/pkg/controller/serviceaccount/tokens_controller.go):
* check how the controller is being created - focus on the event handler
* find the work queue and check how a new key is added there
* find the entry point to the controller that starts it
* see how the workqueue key is being processed

---

-> # Library-go controllers <-

---

## Controller factory
* contains a lot of boilerplate code to set up a controller
    - hooks up the informers to the controller's event handler
    - prepares the queue and its key function
        - *forces the queue keys to be strings!*
    - allows a simplistic view of _bring your own *Sync()* function_

---

## Controller factory fine tuning
* it is possible to fine-tune the basic controller the factory creates
    - filter informer events
    - hook in your own queue key functions
    - schedule regular resync
    - make the controller go degraded on error

---

## * Homework 2 *

1. find out how the things from homework 1 are hooked up
in the library-go controller factory
2. see what things can be tuned on top of the basic
library-go controller factory controller

---

## Demo time

_service-ca operator's secret creating controller_
* different queue functions per different resource

_service-ca operator's CA bundle injector_
* filtering events based on event object's annotation

---

-> # Thanks for your attention and don't forget to start your informers! <-
