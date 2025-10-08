# 📁 Pasta de Evidências - TechFleet

Esta pasta contém todas as evidências/prints do projeto.

## 📋 Prints necessários (nomear exatamente assim):

1. `01-cluster-info.png` - Saída do `kubectl cluster-info`
2. `02-namespace.png` - Saída do `kubectl get namespace producao`
3. `03-recursos-gerais.png` - Saída do `kubectl get all -n producao`
4. `04-pods-iniciais.png` - Saída do `kubectl get pods -n producao -l app=app-portal` (3 réplicas)
5. `05-service.png` - Saída do `kubectl get svc -n producao`
6. `06-antes-scale.png` - Antes do scale (3 réplicas)
7. `07-depois-scale.png` - Depois do scale (5 réplicas)
8. `08-antes-exclusao.png` - Antes da exclusão do pod
9. `09-depois-recuperacao.png` - Depois da recuperação do pod
10. `10-aplicacao-funcionando.png` - (Opcional) Teste curl ou browser

## 🎯 Como fazer:

1. Execute o comando
2. Tire print da saída
3. Salve na pasta `docs/` com o nome correto
4. O README será atualizado automaticamente

## 📂 Estrutura final:
```
docs/
├── 01-cluster-info.png
├── 02-namespace.png
├── 03-recursos-gerais.png
├── 04-pods-iniciais.png
├── 05-service.png
├── 06-antes-scale.png
├── 07-depois-scale.png
├── 08-antes-exclusao.png
├── 09-depois-recuperacao.png
└── 10-aplicacao-funcionando.png
```