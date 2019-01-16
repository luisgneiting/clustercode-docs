Architecture Decisions
----------------------

This chapter should explain why certains decisions were made in order to help
understanding possible future issues.

Microservices: Communication model
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. list-table::

  * - **Problem**
    - Splitting the business logic into several microservices imposes new
      challenges on how to communicate instructions between the components.
  * - **Alternatives**
    - * Shared Database: Similar to a whiteboard pattern, the master saves
        tasks in database. Those informations will be picked up by workers
        when they have finished their previous task. Eventually the workers are
        polling the DB
      * REST: Worker and Master communicate via a REST interface. Since the
        communication needs to be bidirectional, the use of websockets is
        required. Otherwise two services query eachother and that leads to
        circular depedencies.
      * gRPC: A popular contract-based, binary protocol that
        supports bidirectional communication, but broadcasting requires
        maintaining connections to all participating members.
      * Messaging: Central messaging may be more complex to setup, but is
        better suitable for event-based communication patterns. Moreover, a
        sender does not need to know its receivers, thus removing the need for
        any service discoveries as long as every member knows the messaging
        server.
  * - **Decision**
    - Messaging
  * - **Rationale**
    - It is easier to broadcast and do publish/subscribing with messaging than
      implement this behaviour using polling databases or through http. We can
      use what the messaging already provides and focus more on business logic.


API: Message format
^^^^^^^^^^^^^^^^^^^

.. list-table::

  * - **Problem**
    - The microservices need a well-defined message format to understand
      eachother.
