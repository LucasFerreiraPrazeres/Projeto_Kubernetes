# Projeto_Kubernetes

### **Pré-requisitos:**
- Rodar um cluster Kubernetes localmente usando Kind com 2 workers e 1 master.</li>
  
    
### **Tasks**
  
 - [x] Crie um Pod da imagem httpd:2.4.41-alpine no namespace default. O Pod deve ser
      nomeado pod1 o container deve ter o nome pod1-container. O Pod deverá ser
      somente agendado no nó master (control-plane). Não adicione novos labels em
      nenhum dos nós existentes e nem use nodeName no pod. Explique a razão pelo
      qual o pod não é agendado inicialmente no nó master (apenas nos workers) e o
      que precisou ser feito para fazer ele ser agendado no master.

- [x] Crie um namespace chamado "virtualizacao". Nele, crie um Deployment chamado
      "deploy-importante" com o label "id=muito-importante" (os pods devem também
      ter esse label) e 3 réplicas. O pod deve conter dois containers, o primeiro container
      chamado "container1" usando a imagem nginx:1.17.6-alpine e o segundo container
      chamado "container2" com a imagem "kubernetes/pause".
      Deve haver apenas um pod do deployment rodando em cada nó worker.
      Considerando que o ambiente possui apenas 2 nós workers, o pod referente à
      terceira réplica não deverá ser agendado (permanecerá no estado Pending), até
      que um novo nó seja adicionado.

- [x] Crie um novo PersistentVolume chamado virt-pv. Ele deve ter a capacidade de
      2GB, modo de acesso ReadWriteOnce e apontar para o diretório local usando
      hostPath para /Volumes/data.
      Em seguida, crie uma nova PersistentVolumeClaim no namespace "virtualizacao"
      chamado "virt-pvc". Ela deve requerer 2Gi de storage usar accessMode
      ReadWriteOnce. A PVC deverá se vincular (fazer Bound) com a PV corretamente.
      Por fim, crie um novo Deployment no namespace "virtualizacao" que monta o
      volume criado no diretório /tmp/data. Os pods do deployment devem rodar a
      imagem httpd:2.4.41-alpine.
      
 - [ ] Realize o backup do etcd no nó control plane do cluster e salve ele no arquivo
      /tmp/etcd-backup.db. Em seguida, crie um pod usando a imagem
      nginx:1.17.6-alpine. Finalmente, faça a restauração do backup confirmar que o
      cluster está funcionando corretamente e que o pod que foi criado após a extração
      do backup não está mais presente.
      Para fazer o backup, utilize a ferramenta etcdctl (detalhes sobre como baixar aqui) e
      baixe ela dentro do nó control plane. Ao usar um cluster construído com Kind, o nó
      control plane pode ser acessado através do comando "docker exec -ti
      [nome-container] sh". Para saber o nome do container referente ao control plane,
      use o comando "docker ps -a".
      
 - [x] Crie uma nova ServiceAccount "processor" no namespace "virtualizacao". Crie uma
      Role e RoleBinding, ambos também chamados "processor". Esse roles devem
      permitir que a nova ServiceAccount tenha acesso apenas à criação de Secrets e
      ConfigMaps no referido namespace.
      
 - [x] Instale, usando charts Helm, duas aplicações: wordpress e mysql, no namespace
      virtualizacao. Adicione a ambos os deployments o label "app="wp-virt" (utilizando
      o values.yaml dos charts). Em seguida, suponha que um invasor conseguiu acessar
      todo o seu cluster a partir de um pod criado no deployment "deploy-importante"
      que foi aplicado na questão 2. Para evitar esse problema, crie uma NetworkPolicy
      chamada np-wp-virt no namespace virtualizacao, que deverá permitir que apenas
      os pods contendo o label "app=wp-virt" tenham acesso a conectar no pod do
      mysql na porta 3306.
      Depois da implementação, tente realizar conexões a partir de outros pods (como o
      pod do deploy-importante) para o pod do Mysql na porta 3306 e verifique que a o
      acesso não irá mais funciona.
