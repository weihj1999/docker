# CloudTest

## 业务北京

随着云扩容需求激增，通常的验收测试效率已经不能满足诉求，继续打通从研发到生产环境的自动化能力共享通道，在研发区能直接部署现网自动化，利用自动化进行扩容局点的验收测试。
![cloudtest arch](images/cloudtest-001.jpeg)
- 基于APITest，测试框架归一化、测试环境标准化，不同局点复用测试环境及脚本，实现资源最大化利用:
![cloudtest arch](images/cloudtest-002.jpeg)
- 实践DNA2-突破内外网隔离：
基于APITest跨区域融合测试方案，打通研发内外网测试工具协同通道，构建短平快测试能力，基于任务环境变量可配置方式实现单个用例在多个服务、多个局点零成本在线复用，已在华为云、TLF合营云验收测试使用，验收测试周期由15天缩短至7天，效率提升1倍，实现类生产与生产环境秒级切换
![cloudtest arch](images/cloudtest-003.jpeg)
- 实践DNA3-备案网络跨区访问场景：
复用测试脚本和执行器，需跨网络区域访问，涉及信息安全风险，通过设计具体跨网络区域访问场景，并在产品线信息安全部门进行评审备案，扫清信息安全障碍.
![cloudtest arch](images/cloudtest-004.jpeg)
- 落地效果-CloudBU解决方案基于APITest实现验收测试框架统一化，测试环境标准化，成本降低3倍，合营云25个服务325个验收测试用例自动化，已在华为云、TLF合营云验收测试使用，验收测试周期由15天缩短至7天，效率提升1倍；
![cloudtest arch](images/cloudtest-005.jpeg)
- 落地效果（优秀DNA复制）—云核视频PaaS 媒体云服务基于APITest实现3个服务434个OpenAPI接口测试用例自动化，通过配置VPN方案直接对蓝区公有云类生产环境进行接口验收看护，提升验收测试效率XX倍
![cloudtest arch](images/cloudtest-006.jpeg)
