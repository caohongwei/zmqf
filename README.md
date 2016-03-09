# zmqf
zmq based web framework

# Install
#### 1. Compile And Install zmq
[http://zeromq.org/](http://zeromq.org/ "official web site")

#### 2. Install zmqf
```bash
$ pip install zmqf
```

# Example
#### 1. server
```PYTHON
  from zmqf.core.base import ZmqfHandler, ZmqfApplication, ZmqfServer

  class MainHanlder(ZmqfHandler):
      '''
      request handler
      '''
      
      def __init__(self, *args, **kwargs):
          ZmqfHandler.__init__(self, *args, **kwargs)
          
      def handle(self):
          print self.request.uri, self.request.headers, self.request.body
  
  
  class App(ZmqfApplication):
      '''
      web application
      '''
      
      def __init__(self):
          
          
          ZmqfApplication.__init__(self, 
                               handlers = [
                                           (r'/', MainHanlder),
                                           (r'/v1/', MainHanlder)
                                           ]
                               )
          
  def main(broker='tcp://127.0.0.1:5556'):
      ZmqfServer(App(), broker).start()
  
  if __name__ == '__main__':
      main()
```
#### 2. broker
```PYTHON
  from zmqf.core.broker import ZmqfBroker
  
  def main(frontend='tcp://*:5555', backend='tcp://*:5556'):
      ZmqfBroker(frontend, backend).start()
      
  if __name__ == '__main__':
      main()
```
#### 3. client
```PYTHON
  from zmqf.client import mpbs
  
  def main(server_addr="tcp://localhost:5555"):
      
      mpbs.request(server_addr, json={"a": 1}, headers={"cmdid": 10000})
  
  if __name__ == '__main__':
      main()
```
