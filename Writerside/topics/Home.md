[MiguelColl]: https://github.com/MiguelColl

[LtVish]: https://github.com/LtVish

[DanielAlmazan]: https://github.com/DanielAlmazan

[Nest Hotel]: https://github.com/DanielAlmazan/hotel-nest

[TaskLynx Business]: https://github.com/DanielAlmazan/TaskLynx-JavaFX

[TaskLynx API]: https://github.com/DanielAlmazan/TaskLynx-SpringBoot

[TaskLynx Mobile]: https://github.com/DanielAlmazan/TaskLynx-Mobile

# TaskLynx / Nest Hotel – Docs

![TaskLynx-logo-oval.svg](TaskLynx-logo-oval.svg)

## Introduction

TaskLynx is an ecosystem composed of TaskLynx Api, TaskLynx Business and TaskLynx Mobile.
[TaskLynx Api] is a REST API that allows communication between [TaskLynx Business] and [TaskLynx Mobile].

[Nest Hotel] is a REST API that allows communication with [TaskLynx Business] in order to manage
the cleaning of hotel rooms.

[TaskLynx Mobile] is a mobile application that allows hotel staff to manage the cleaning of hotel rooms.
It communicates with [TaskLynx Api] to get the tasks to be done.  
"Authentication" is done by using the endpoint `/api/trabajadores/{idTrabajador}/{contraseña}`. Didn't use JWT
because it was not required.

[TaskLynx Business] is a desktop application that allows hotel managers to manage the cleaning of hotel rooms and other
tasks. It communicates with [TaskLynx Api] to get the tasks to be done and with [Nest Hotel] to manage the rooms.
