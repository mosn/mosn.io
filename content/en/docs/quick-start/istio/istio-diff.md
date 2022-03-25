bin/init.sh

```diff
-WASM_RELEASE_DIR=${ISTIO_ENVOY_LINUX_RELEASE_DIR}
-for plugin in stats metadata_exchange
-do
-  FILTER_WASM_URL="${ISTIO_ENVOY_BASE_URL}/${plugin}-${ISTIO_ENVOY_VERSION}.wasm"
-  download_wasm_if_necessary "${FILTER_WASM_URL}" "${WASM_RELEASE_DIR}"/"${plugin//_/-}"-filter.wasm
-  FILTER_WASM_URL="${ISTIO_ENVOY_BASE_URL}/${plugin}-${ISTIO_ENVOY_VERSION}.compiled.wasm"
-  download_wasm_if_necessary "${FILTER_WASM_URL}" "${WASM_RELEASE_DIR}"/"${plugin//_/-}"-filter.compiled.wasm
-done
+#WASM_RELEASE_DIR=${ISTIO_ENVOY_LINUX_RELEASE_DIR}
+#for plugin in stats metadata_exchange
+#do
+#  FILTER_WASM_URL="${ISTIO_ENVOY_BASE_URL}/${plugin}-${ISTIO_ENVOY_VERSION}.wasm"
+#  download_wasm_if_necessary "${FILTER_WASM_URL}" "${WASM_RELEASE_DIR}"/"${plugin//_/-}"-filter.wasm
+#  FILTER_WASM_URL="${ISTIO_ENVOY_BASE_URL}/${plugin}-${ISTIO_ENVOY_VERSION}.compiled.wasm"
+#  download_wasm_if_necessary "${FILTER_WASM_URL}" "${WASM_RELEASE_DIR}"/"${plugin//_/-}"-filter.compiled.wasm
+#done
```

bin/update_proxy.sh

```diff
-WASM_URL=${ISTIO_ENVOY_BASE_URL}/${plugin}-${ISTIO_ENVOY_VERSION}.wasm
-printf "Verifying %s is available\n" "$WASM_URL"
-until curl --output /dev/null --silent --head --fail "$WASM_URL"; do
-    printf '.'
-    sleep $SLEEP_TIME
-done
+#WASM_URL=${ISTIO_ENVOY_BASE_URL}/${plugin}-${ISTIO_ENVOY_VERSION}.wasm
+#printf "Verifying %s is available\n" "$WASM_URL"
+#until curl --output /dev/null --silent --head --fail "$WASM_URL"; do
+#    printf '.'
+#    sleep $SLEEP_TIME
+#done
```

pilot/docker/Dockerfile.proxyv2

```diff
-COPY stats-filter.wasm /etc/istio/extensions/stats-filter.wasm
-COPY stats-filter.compiled.wasm /etc/istio/extensions/stats-filter.compiled.wasm
-COPY metadata-exchange-filter.wasm /etc/istio/extensions/metadata-exchange-filter.wasm
-COPY metadata-exchange-filter.compiled.wasm /etc/istio/extensions/metadata-exchange-filter.compiled.wasm
+#COPY stats-filter.wasm /etc/istio/extensions/stats-filter.wasm
+#COPY stats-filter.compiled.wasm /etc/istio/extensions/stats-filter.compiled.wasm
+#COPY metadata-exchange-filter.wasm /etc/istio/extensions/metadata-exchange-filter.wasm
+#COPY metadata-exchange-filter.compiled.wasm /etc/istio/extensions/metadata-exchange-filter.compiled.wasm
```

tools/istio-docker.mk

```diff
 # rule for wasm extensions.
-$(ISTIO_ENVOY_LINUX_RELEASE_DIR)/stats-filter.wasm: init
-$(ISTIO_ENVOY_LINUX_RELEASE_DIR)/stats-filter.compiled.wasm: init
-$(ISTIO_ENVOY_LINUX_RELEASE_DIR)/metadata-exchange-filter.wasm: init
-$(ISTIO_ENVOY_LINUX_RELEASE_DIR)/metadata-exchange-filter.compiled.wasm: init
+#$(ISTIO_ENVOY_LINUX_RELEASE_DIR)/stats-filter.wasm: init
+#$(ISTIO_ENVOY_LINUX_RELEASE_DIR)/stats-filter.compiled.wasm: init
+#$(ISTIO_ENVOY_LINUX_RELEASE_DIR)/metadata-exchange-filter.wasm: init
+#$(ISTIO_ENVOY_LINUX_RELEASE_DIR)/metadata-exchange-filter.compiled.wasm: init

-docker.proxyv2: $(ISTIO_ENVOY_LINUX_RELEASE_DIR)/stats-filter.wasm
-docker.proxyv2: $(ISTIO_ENVOY_LINUX_RELEASE_DIR)/stats-filter.compiled.wasm
-docker.proxyv2: $(ISTIO_ENVOY_LINUX_RELEASE_DIR)/metadata-exchange-filter.wasm
-docker.proxyv2: $(ISTIO_ENVOY_LINUX_RELEASE_DIR)/metadata-exchange-filter.compiled.wasm
+#docker.proxyv2: $(ISTIO_ENVOY_LINUX_RELEASE_DIR)/stats-filter.wasm
+#docker.proxyv2: $(ISTIO_ENVOY_LINUX_RELEASE_DIR)/stats-filter.compiled.wasm
+#docker.proxyv2: $(ISTIO_ENVOY_LINUX_RELEASE_DIR)/metadata-exchange-filter.wasm
+#docker.proxyv2: $(ISTIO_ENVOY_LINUX_RELEASE_DIR)/metadata-exchange-filter.compiled.wasm
```

pkg/config/constants/constants.go

```diff
-       BinaryPathFilename = "/usr/local/bin/envoy"
+       BinaryPathFilename = "/usr/local/bin/mosn"
```
