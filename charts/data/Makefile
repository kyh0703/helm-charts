default: dep lint test

TARGET          = $(shell basename $(PWD))

CA_CERT         := $(if $(CA_CERT),$(CA_CERT),'../../ca.crt')
HELM_REPO_USR   := $(if $(HELM_REPO_USR),$(HELM_REPO_USR),'admin')
HELM_REPO_PSW   := $(if $(HELM_REPO_PSW),$(HELM_REPO_PSW),'12341234')
PRIVATE_REPO    := $(if $(PRIVATE_REPO),$(PRIVATE_REPO),"https://127.0.0.1/chartrepo/core")
BUILD_ENV       := $(if $(BUILD_ENV),$(BUILD_ENV),'dev')
BUILD_DATE      := $(if $(BUILD_DATE),$(BUILD_DATE),$(shell date '+%Y-%m-%dT%H:%M:%S'))

CURRENT_DIR     = .
VERSION         = $(shell cat $(CURRENT_DIR)/VERSION)
CHART_PATH      = $(CURRENT_DIR)/$(VERSION)
ENV_VALUE       = ../../helm-values/$(BUILD_ENV).yaml
VARIANT_VALUE   = $(CHART_PATH)/variants/$(BUILD_ENV)/values.yaml

.PHONY: dep
dep:
	@echo "dep:"
	@helm dep up $(CHART_PATH)
	@echo ""

.PHONY: lint
lint:
	@echo "lint:"
	@helm lint --with-subcharts --debug \
		--values $(ENV_VALUE) \
		--values $(VARIANT_VALUE) \
		$(CHART_PATH)
	@echo ""

.PHONY: view
view:
	@echo "view:"
	@helm template --validate \
		--values $(ENV_VALUE) \
		--values $(VARIANT_VALUE) \
		$(CHART_PATH)
	@echo ""

.PHONY: test
test:
	@echo "test:"
	@helm install --dry-run \
		--values $(ENV_VALUE) \
		--values $(VARIANT_VALUE) \
		$(TARGET) $(CHART_PATH)
	@echo ""

.PHONY: install
install:
	@echo "install:"
	@helm install $(TARGET) \
		--values $(ENV_VALUE) \
		--values $(VARIANT_VALUE) \
		$(CHART_PATH)
	@echo ""

.PHONY: push
push:
	@echo "push:"
	@helm cm-push \
		--ca-file $(CA_CERT) \
		--username $(HELM_REPO_USR) \
		--password $(HELM_REPO_PSW) \
		$(CHART_PATH) $(PRIVATE_REPO)
	@echo ""