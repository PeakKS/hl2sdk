proto_files = [
  'connectionless_netmessages.proto',
  'engine_gcmessages.proto',
  'netmessages.proto',
  'network_connection.proto',
  'networkbasetypes.proto',
  'valveextensions.proto',
]

foreach proto_file : proto_files
  proto_gen_src += proto_gen.process(proto_file)
endforeach