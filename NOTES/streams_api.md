# Streams in Java

## What is Streams
- Streams were introduced in **Java 1.8**.  
- Streams represent a **sequence of data elements** that can support **aggregate operations** in **sequential** and **parallel** mode.  
- Streams enable efficient and readable data processing using **functional** and **declarative** approaches.  

- **Streams are not data structures**, meaning in streams you **cannot store data**, but you can perform operations on the data.  
- **Streams are APIs.** For streams, memory is not allocated. By default, streams enable **sequential** execution, not parallel.  

- Since streams do not store data, they **do not hold memory** for data. However, for operations, streams require memory provided by the **heap area** if it is sequential; if it is parallel, the **heap and thread stack** are used. Once an operation completes, the **garbage collector** immediately cleans the stream operation space, making it **memory efficient**.  

- In Streams, **intermediate operations** are optional, but **terminal operations** are mandatory.  
  Streams are **single-directional (forward directional)**, not bi-directional.  
  Multiple intermediate operations can be combined and performed.  

- **Stream flow:**  
  `Stream Source (Collections, Arrays) → Intermediate Operations → Terminal Operations`  
  Once you provide terminal operations, the stream executes.  
  After terminal operations complete, the stream cannot be reused — it is the **end of the stream**.  

---

## Stream Sources
- Collections  
- Arrays  
- Strings  
- Primitives  
- Files  
- Streams from I/O (Input and Output streams — *I/O streams and Java streams are different*)  
- Generators  
- Stream Builder  

---

## Intermediate Operations
- `filter`  
- `map`  
- `flatMap`  
- `distinct`  
- `sorted`  
- `peek`  
- `limit`  
- `skip`  

---

## Terminal Operations
- `forEach`  
- `toArray`  
- `collect`  
- `min`  
- `max`  
- `count`  
- `anyMatch`  
- `allMatch`  
- `noneMatch`  
- `findFirst`  
- `findAny`  
- `reduce`  

---

## Features of Streams
- Functional Programming  
- Concise and Readable Code  
- Parallel Processing  
- Efficient Data Processing  
- Reduced Errors  
- Modern Java Development  
  


## Pictorial Representation

<img width="1919" height="1061" alt="Screenshot 2025-11-04 153516" src="https://github.com/user-attachments/assets/99dfbc9d-efc7-4a61-a8d1-ba7433fb8a24" />
