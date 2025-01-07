# Acesso a máquina privada por meio de Agent Forwarding

É uma técnica que permite que você use a chave privada armazenada no seu computador local para autenticar em uma instância privada (na subnet privada), passando pelo bastion host, sem a necessidade de transferir ou copiar a chave privada para o bastion host. No final do material vamos ter algumas dicas para o uso dessa tecnica.

1. Abra o PowerShell como administrador
2. Execute o comando: `ssh-add .\chave.pem` (você deve estar dentro do diretório da sua chave ou indicar o caminho correto para a chave*)
3. Para verificar se a chave foi adicionada execute o comando: `ssh-add -l` (a letra após o - é L em caixa baixa)
4. Agora, acesse a máquina pública com o seguinte comando `ssh -A -i "NOMECHAVE.pem" ec2-user@IPDAMAQUINAPUBLICA` esse -A no comando vai permitir acessar a máquina "levando" a chave que copiamos no Agent Forwarding
5. dentro da máquina pública vamos executar o seguinte comando para acessar a máquina privada: `ssh ec2-user@IPMAQUINAPRIVADA` note que foi removido o -i e a chave
![resultado esperado: note que acessamos a máquina pública e ele mostra o ip dessa maquina e rodamos o ssh para acessar a máquina privada](https://github.com/user-attachments/assets/34937140-ddaa-4a0e-840a-0bf539b4c307)

 Nesse momento todo comando executado será executado dentro da máquina privada.


## Agent Forwarding dicas


1. para saber se o OpenSSh esta instalado na sua máquina execute o comando:
    Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'

2. instalar o OpenSSh caso não esteja instalado:
    dism /Online /Add-Capability /CapabilityName:OpenSSH.Client~~~~0.0.1.0

3. verificar se o OpenSSH esta executando na sua máquina:
    Get-Service ssh-agent

4. iniciar o serviço OpenSSH:
    Start-Service ssh-agent
