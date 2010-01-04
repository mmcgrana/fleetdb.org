---
layout: page
title: "Docs : Protocol"
docs_tab: "p"
---

FleetDB clients communicate with servers over TCP using a simple [JSON](http://json.org)-based protocol.

A client first connects to a server via a TCP socket on the port specified at server startup, which by default is 3400. The client then executes one or more request/response cycles. A client makes a request by writing a JSON string representing the [query](/docs/queries.html), followed by a CRLF. The client then reads the CRLF-terminated JSON response.

The parsed response will be an array of 2 elements: a status code and response value. A status code of 0 indicates success, in which case the response value is the result for the query. A positive status code indicates an error, in which case the response value is an error message.

Perhaps the best way to understand the protocol is to see the source for an actual implementation. For example, here is a simplified version of how the the [Ruby client](/docs/clients/ruby.html) implements its main `query` method:

    def query(q)
      request = @json_encoder.encode(q)
      @socket.write(request)
      @socket.write("\r\n")
      response = @socket.gets
      status, value = @json_parser.parse(response)
      status == 0 ? value : raise(value)
    end
