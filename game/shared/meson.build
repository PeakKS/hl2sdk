proto_files = [
  'base_gcmessages.proto',
  'clientmessages.proto',
  'gameevents.proto',
  'te.proto',
  'usercmd.proto',
  'usermessages.proto',
]

foreach proto_file : proto_files
  sdk_proto += proto_gen.process(proto_file)
endforeach

subdir('cs')
subdir('cstrike15')
# subdir('econ')