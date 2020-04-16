
IMAGE_NAME="rez/base:$(rez_version)"
USER_ID := $(shell id -u)
GROUP_ID := $(shell id -g)
USER_NAME := $(shell id -un)
GROUP_NAME := $(shell id -gn)


build:
ifdef rez_version
	docker build -t $(IMAGE_NAME) \
		 --build-arg USER_ID=$(USER_ID) \
		 --build-arg GROUP_ID=$(GROUP_ID) \
		 --build-arg USER_NAME=$(USER_NAME) \
		 --build-arg REZ_VERSION=$(rez_version) \
		 $(PWD)
else
	@echo "rez_version not define..."
endif


run:
	docker run -it \
		-u $(USER_NAME):$(GROUP_NAME) \
		--mount type=bind,source=/datas/$(USER_NAME),target=/datas/$(USER_NAME) \
		--mount type=bind,source=/s/apps/packages,target=/s/apps/packages \
		$(IMAGE_NAME) /bin/bash


clean:
	docker rmi -f $(IMAGE_NAME)
