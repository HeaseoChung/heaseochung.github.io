# Core

## class | tensorrt.INetworkDefinition

- 빌더가 엔진을 빌드하기 위한 TensorRT 네트워크
- 네트워크에 layer를 추가적으로 추가 가능. 예) add_softmax
- 네트워크의 layer, input, output 등의 정보를 가져올 수 있음

## class | tensorrt.ICudaEngine

- ICudaEngine은 구축된 네트워크에서 추론을 실행하기 위한 클래스
- 엔진은 []로 인덱스가 가능하며, int 인덱스의 경우 관련된 바인딩 이름을 반환하고 str 인덱스의 경우 관련된 바인딩 인덱스를 반환

## class | tensorrt.IOptimizationProfile

- Dynamic input의 dimensions 그리고 shape tensor에 대한 Optimization profile를 설정하는 클래스

## class | tensorrt.IBuilderConfig

- 빌드에 필요한 configurations 정의하는 클래스
- Quantization, Calibration, Dynamic shape, device 등의 설정을 포함하고 있음

## class | tensorrt.Builder

- INetworkDefinition에서 ICudaEngine을 빌드하기 위한 클래스

### func(network, config) | build_engine

- 주어진 INetworkDefinition 와 IBuilderConfig로 engine을 빌드하는 함수

## func(logger, namespace) -> bool | tensorrt.init_libnvinfer_plugins

- 기존의 tensorrt plugins 을 IpluginRegistry에 namespace로 초기화 또는 등록
