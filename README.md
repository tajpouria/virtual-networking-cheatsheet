```sh
ip netns add A
ip netns add B
ip netns ls

ip link add br0 type bridge
ip link set br0 up
ip link show br0
ip link

ip link add veth1 type veth peer name veth1-br
ip link add veth2 type veth peer name veth2-br
ip link show veth1
ip link show veth1-br

ip link set veth1 netns A
ip link set veth2 netns B
ip netns A exec ip link
ip netns B exec ip link

ip link set veth1-br master br0
ip link set veth2-br master br0

ip link set veth1-br up
ip link set veth2-br up
ip netns exec A ip link set veth1 up
ip netns exec B ip link set veth2 up

ip netns exec A ip addr add 10.0.0.1/24 dev veth1
ip netns exec B ip addr add 10.0.0.1/24 dev veth2
ip netns exec A ip addr show veth1
ip netns exec B ip addr show veth2

ip netns exec A ping 10.0.0.2
ip netns exec B ping 10.0.0.1

# bring up loopback link to ping itself
ip netns exec A ip link set lo up
ip netns exec A ping 10.0.0.1
```
