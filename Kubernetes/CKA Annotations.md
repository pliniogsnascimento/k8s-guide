```bash
alias kg="kubectl get"
alias kgpo="kubectl get pods"
alias kgdep="kubectl get deploy"
alias krm="kubectl delete"
alias krmpo="kubectl delete pods --grace-period=0 --force"
alias krmpoall="krmpo --all"
alias ka="k apply -f"
alias kns="kubectl config set-context --current --namespace"
export drc="--dry-run=client -oyaml"
export img="-ojsonpath-as-json={.spec.containers..image}"
```