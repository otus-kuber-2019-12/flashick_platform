- Реализована сборка и выгрузка образов на докерхаб с помощью gitlab-ci
- Реализована раскатка релизов хельма  из репы на гитлабе через flux
- Реализована канареечная раскатка с поvощью fragger


Строки в логах, говорящие об изменениях выглядят несколько странно:

ts=2020-03-07T12:33:40.381216804Z caller=release.go:398 component=release release=frontend targetNamespace=microservices-demo resource=microservices-demo:helmrelease/frontend helmVersion=v3 info="chart has diverged" diff="  &helm.Chart{\n  \t... // 2 identical fields\n  \tAppVersion: \"1.16.0\",\n  \tValues:     helm.Values{\"environment\": string(\"develop\"), \"image\": map[string]interface{}{\"repository\": string(\"flashick/frontend\"), \"tag\": string(\"latest\")}, \"ingress\": map[string]interface{}{\"host\": string(\"develop.kubernetes-platform-demo.express42.io\")}},\n  \tTemplates: []*helm.File{\n  \t\t... // 2 identical elements\n  \t\t&{Name: \"templates/service.yaml\", Data: []uint8{0x61, 0x70, 0x69, 0x56, 0x65, 0x72, 0x73, 0x69, 0x6f, 0x6e, 0x3a, 0x20, 0x76, 0x31, 0x0a, 0x6b, 0x69, 0x6e, 0x64, 0x3a, 0x20, 0x53, 0x65, 0x72, 0x76, 0x69, 0x63, 0x65, 0x0a, 0x6d, 0x65, 0x74, 0x61, 0x64, 0x61, 0x74, 0x61, 0x3a, 0x0a, 0x20, 0x20, 0x6e, 0x61, 0x6d, 0x65, 0x3a, 0x20, 0x66, 0x72, 0x6f, 0x6e, 0x74, 0x65, 0x6e, 0x64, 0x0a, 0x20, 0x20, 0x6c, 0x61,
......
ts=2020-03-07T12:33:40.79291783Z caller=helm.go:69 component=helm version=v3 info="Created a new Deployment called \"frontend-hipster\" in microservices-demo\n" targetNamespace=microservices-demo release=frontend
ts=2020-03-07T12:33:40.873855966Z caller=helm.go:69 component=helm version=v3 info="Deleting \"frontend\" in microservices-demo..." targetNamespace=microservices-demo release=frontend