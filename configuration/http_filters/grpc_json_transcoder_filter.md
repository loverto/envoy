# gRPC-JSON 转码

- gRPC [architecture overview](../../intro/arch_overview/grpc.md#arch-overview-grpc)
- [v1 API reference](../../api-v1/http_filters/grpc_json_transcoder_filter.md#config-http-filters-grpc-json-transcoder-v1)
- [v2 API reference](../../api-v2/config/filter/http/transcoder/v2/transcoder.proto.md#envoy-api-msg-config-filter-http-transcoder-v2-grpcjsontranscoder)

This is a filter which allows a RESTful JSON API client to send requests to Envoy over HTTP and get proxied to a gRPC service. The HTTP mapping for the gRPC service has to be defined by [custom options](https://cloud.google.com/service-management/reference/rpc/google.api#http).

## How to generate proto descriptor set

Envoy has to know the proto descriptor of your gRPC service in order to do the transcoding.

To generate a protobuf descriptor set for the gRPC service, you’ll also need to clone the googleapis repository from GitHub before running protoc, as you’ll need annotations.proto in your include path, to define the HTTP mapping.

```
git clone https://github.com/googleapis/googleapis
GOOGLEAPIS_DIR=<your-local-googleapis-folder>
```

Then run protoc to generate the descriptor set from bookstore.proto:

```
protoc -I$(GOOGLEAPIS_DIR) -I. --include_imports --include_source_info \
  --descriptor_set_out=proto.pb test/proto/bookstore.proto
```

If you have more than one proto source files, you can pass all of them in one command.