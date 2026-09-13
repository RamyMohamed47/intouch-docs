# Data Flow Diagram - Level 1

~~~mermaid
flowchart TD
  Clients[Web and mobile clients] --> Auth[Authentication]
  Clients --> Org[Organization and membership]
  Clients --> Chat[Conversation and messaging]
  Clients --> Voice[Voice and call lifecycle]
  Clients --> AI[Echo and search]
  Auth --> Mongo[(MongoDB)]
  Org --> Mongo
  Chat --> Mongo
  Voice --> Mongo
  AI --> Mongo
  Auth --> Google[Google identity]
  Chat --> R2[Private R2]
  Chat --> Redis[(Redis/BullMQ)]
  Voice --> Redis
  Voice --> LiveKit[LiveKit media]
  AI --> Gemini[Gemini]
  Org --> Delivery[Mail/Expo push]
~~~
