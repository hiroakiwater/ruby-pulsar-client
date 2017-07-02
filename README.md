# ruby-pulsar-client
client code for pulsar(https://github.com/apache/incubator-pulsar).

# Usage

This code is using following gems.

```
gem install digest-crc
```

```
gem install ruby_protobuf
```

# Setup

## Compile Pulsar API protocol buffer

1. Clone proto file(PulsarApi.proto) from Pulsar Project page.
```
git clone https://github.com/apache/incubator-pulsar.git
```
(The proto file path is at pulsar-common/src/main/proto/PulsarApi.proto)


2. Compile the proto file using rprotoc
```
rprotoc PulsarApi.proto
```

3. Move PulsarApi.pb.rb to your project directory.

# Examples

## Producer
```
require './ruby-pulsar-client/lib/PulsarClient'

client = Message::PulsarClient.new()
client.connect('localhost', 6650)

client.send('persistent://sample/standalone/ns1/my-topic', 'hello!')
client.close()
```


## Consumer
```
require './ruby-pulsar-client/lib/PulsarClient'

client = Message::PulsarClient.new()
client.connect('localhost', 6650)

client.subscribe('persistent://sample/standalone/ns1/my-topic', 'sub', 1)
while true do
        m = client.get_message()
        print(m.message)
        print("\n")
        client.ack(m.client_created_id, m.message_ledger_id, m.message_entry_id)
end
```


