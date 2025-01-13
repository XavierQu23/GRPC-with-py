
# gRPC Simple Example in Python

This project is a basic example of a gRPC implementation in Python. The service allows sending a name to the server and receiving a personalized greeting as a response.

## Project Structure

gRPC_Simple/ │ ├── greeting.proto # gRPC service definition ├── server.py # gRPC server code └── client.py # gRPC client code


### Description of Files

1. **greeting.proto**:
   - This file defines the gRPC service and the messages that will be exchanged between the client and the server.
   - It contains the definition of the `GreetingService` with a `SayHello` method, which takes a `HelloRequest` message and returns a `HelloResponse` message.
   - The `.proto` file needs to be compiled to generate the necessary Python files.

2. **server.py**:
   - This file contains the code for the gRPC server. The server implements the logic for the service defined in the `.proto` file.
   - The server listens on port `50051` and waits for requests from the client.
   - When the server receives a request, it responds with a personalized greeting based on the name sent by the client.

3. **client.py**:
   - This file contains the code for the gRPC client. The client sends a request to the server with the user's name and receives a greeting in response.
   - The client connects to the server at `localhost:50051`, sends the message, and displays the server's response in the terminal.

## Requirements

Make sure to have the following installed:

- Python 3.6 or higher.
- The necessary packages (`grpcio` and `grpcio-tools`), which can be installed by running:

```bash
pip install grpcio grpcio-tools 
```

Running Instructions
1. Define the service and generate Python files
First, you need to generate the Python files from the greeting.proto file. To do this, run the following command in your terminal:
```python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. greeting.proto```

This will generate two files:

1. greeting_pb2.py: Contains the definitions of the messages (HelloRequest, HelloResponse).
greeting_pb2_grpc.py: Contains the definitions of the gRPC service (GreetingService).

2. Start the server
To start the server, run the following command in your terminal:

      ```python server.py```

This will start the gRPC server on port 50051.

3. Run the client
In another terminal, run the following command to start the client:
```python client.py```

The client will send the name Alice to the server and display the response message: Hello, Alice!.

4. Expected Output
When you run the client, you should see something like this in the client terminal:
```Server responded: Hello, Xavi!```

And in the server terminal, you should see something like:
```Server running on port 50051...```

Code Explanation
greeting.proto
This file defines the gRPC service and the messages. We use Protocol Buffers to define the service and messages independently of the language, and then compile them to generate code in the desired language (in this case, Python).
```service GreetingService {
  rpc SayHello (HelloRequest) returns (HelloResponse);
}

message HelloRequest {
  string name = 1;
}

message HelloResponse {
  string message = 1;
}
```

1. service GreetingService: Defines the service provided by the server. In this case, it has a single method, SayHello.
2. rpc SayHello: This is the method definition that takes a HelloRequest and returns a HelloResponse.
3. message HelloRequest: Defines the request message, which has a name field of type string.
4. message HelloResponse: Defines the response message, which has a message field of type string.

# server.py

The gRPC server implements the service defined in the greeting.proto file. It uses the GreetingServiceServicer class to define the service logic. In the case of the SayHello method, it returns a greeting message based on the name received in the request.

```class GreetingService(greeting_pb2_grpc.GreetingServiceServicer):```
    ```def SayHello(self, request, context):
        return greeting_pb2.HelloResponse(message=f"Hello, {request.name}!")```

The SayHello method takes a request object, extracts the name, and returns a greeting message with that name.
client.py
The gRPC client connects to the server and sends a request with the name. It then waits for the response and prints it in the terminal.

```with grpc.insecure_channel('localhost:50051') as channel:```
    ```stub = greeting_pb2_grpc.GreetingServiceStub(channel)
    response = stub.SayHello(greeting_pb2.HelloRequest(name="Xavi"))
    print(f"Server responded: {response.message}")```
    
1. The client creates a secure channel (insecure_channel) to communicate with the server on port 50051.
2. It then uses a stub of the service (GreetingServiceStub) to call the SayHello method.
3. The client sends a request message with the name Alice, and when it receives the response, it prints it.







