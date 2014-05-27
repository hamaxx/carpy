carpy
=====

Make your application fluent

Collect application metrics in real time with statsd and graphite


Api proposal
------------

    import carpy.config
    import carpy.wrapper.transaction
  
  
    carpy.config.set('statsd_url', 'localhost:1234')
  
  
    @carpy.wrapper.transaction
    def get_user(request):
      return HttpResponse(get_user_data())
  
    
    @carpy.wrapper.function
    def get_user_data():
      pass



Graphite data model
-------------------

    carpy -+
           +-> test_app -+
                         +-> my_server -+
                                        +-> get_user -+
                                                      +-> ok       -> [meam, upper, count]
                                                      +-> error    -> [mean, upper, count]
                                                      +-> apdex    -> [mean]
                                                      +-> children -+
                                                                    +-> get_user_data -+
                                                                                       +-> ok
                                                                                       +-> error
                                                                                       
                         +-> my_server2 +
                                        +-> get_user -+
                                                      +-> ok       -> [mean, upper, count]
                                                      +-> error    -> [mean, upper, count]
                                                      +-> apdex    -> [mean]
                                                      +-> children -+
                                                                    +-> get_user_data -+
                                                                                       +-> ok
                                                                                       +-> error
                                        +-> set_user -+
                                                      +-> ok       -> [mean, upper, count]
                                                      +-> error    -> [mean, upper, count]
                                                      +-> apdex    -> [mean]
                                                      +-> children -+
                                                                    +-> set_user_data -+
                                                                                       +-> ok
                                                                                       +-> error
