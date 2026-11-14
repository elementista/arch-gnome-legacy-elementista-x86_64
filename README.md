# Arch GNOME Legacy Respin (Elementista)

Imagem ISO personalizada (*respin*) baseada no **Arch Linux (x86_64)** com ambiente **GNOME** e instalador gráfico **Calamares**, construída com **Penguins' Eggs**.

O projeto nasceu de uma necessidade prática: entregar um Arch Linux puro, amigável, leve e validado em bancada real para computadores antigos (4 GB de RAM e HD mecânico), sem gambiarras e mantendo o controle total nas mãos do usuário.

---

## ⚠️ 1. Arquitetura Base, Kernels e Boot (Leia Antes de Modificar)

Esta imagem foi testada no limite para funcionar bem de fábrica. Antes de instalar kernels alternativos ou alterar arquivos de inicialização por recomendação de terceiros, entenda como o sistema opera:

### Política de Kernels (`linux-lts` + `linux`)
A ISO entrega dois kernels oficiais do Arch Linux instalados lado a lado:
* **`linux-lts` (Padrão Principal):** Configurado como primeira opção no menu de boot. Oferece suporte de longo prazo, máxima estabilidade e compatibilidade comprovada com controladoras, placas de rede e vídeos antigos.
* **`linux` (Kernel Principal / Secundário):** Disponível dentro de *"Opções avançadas para Arch Linux"* no menu de boot como alternativa de segurança caso algum periférico mais recente exija um kernel mais novo.

> **AVISO IMPORTANTE SOBRE KERNELS DE TERCEIROS:**  
> Não instale kernels modificados (como Zen, XanMod, Liquorix ou compilados manualmente) sem saber gerenciar módulos DKMS e imagens `initramfs`. O suporte desta ISO cobre exclusivamente os dois kernels oficiais (`linux-lts` e `linux`) entregues na matriz. Alterações na estrutura de kernel ou remoção de ambos os núcleos podem impedir o computador de iniciar.

### Gerenciador de Boot (GRUB) — Zona de Máximo Cuidado
O menu de inicialização inclui atalhos úteis de bancada (*Desligar o Computador* e *Reiniciar o Computador*) e carrega automaticamente o microcode do processador.

* **NUNCA edite o arquivo `/boot/grub/grub.cfg` diretamente.** Ele é gerado automaticamente pelo sistema. Qualquer edição manual ou caractere apagado nesse arquivo impede o computador de ligar.
* **Se errar ao editar qualquer arquivo no terminal (`nano`):** Aperte `Ctrl + X` e depois a tecla `N` (Não) para sair sem salvar e manter o sistema intacto.
---

## 💿 2. Instalação e Particionamento (Ext4 vs. Btrfs e Swap)

Ao avançar no instalador gráfico **Calamares**, a escolha correta do particionamento define a velocidade do sistema no dia a dia:

### Sistema de Arquivos: Use Ext4 em HD Mecânico
* **Ext4 (Recomendado para HDs Mecânicos e Máquinas Legadas):** Sistema de arquivos clássico e maduro do Linux. Grava dados de forma direta e sequencial, exigindo menos movimentação do disco e evitando lentidão em segundo plano. **Escolha obrigatória para quem usa HD comum.**
* **Btrfs (Indicado apenas para SSDs):** Utiliza *Copy-on-Write* e tarefas de manutenção em segundo plano que causam fragmentação e lentidão em HDs mecânicos. Use apenas se tiver um SSD e souber gerenciar *snapshots*.

### Memória Swap: Não Crie Partição Swap no Disco
* **Deixe sem partição Swap ("Sem Swap") durante a instalação.**
* Criar Swap tradicional em HD mecânico faz o disco bater 100% de uso e travar o computador quando a memória RAM enche.
* **Solução nativa da ISO (ZRAM):** O sistema já vem configurado de fábrica com **ZRAM (`zstd` na proporção 1:1 da RAM)**, criando uma área de swap compactada dentro da própria memória RAM — dezenas de vezes mais rápida que qualquer HD.

---

## 🧪 3. Escopo de Validação e Testes Reais de Bancada

Os ajustes de memória e compatibilidade gráfica desta ISO foram testados exclusivamente nos seguintes equipamentos físicos (4 GB de RAM e armazenamento legado):

* **Positivo S14BW01** — Intel Celeron (4 GB RAM)
* **Acer Aspire E1-510** — Intel Celeron Bay Trail (4 GB RAM)
* **Samsung RV415** — AMD Série E / Radeon HD 6310 (4 GB RAM)
* **Lenovo ThinkCentre M93p** — Bancada de desenvolvimento e testes contínuos em HD mecânico

> **Nota de Transparência:** Embora a ISO funcione em qualquer computador compatível com `x86_64`, a validação oficial antes da publicação foi realizada estritamente nos modelos acima.

### O Fator Processador e Gráficos (Por que 4 GB de RAM não são todos iguais)
Nos testes práticos, ficou comprovado que a arquitetura da CPU e do chip gráfico faz uma diferença gritante mesmo com a mesma quantidade de RAM:
* **Positivo S14BW01 (Melhor Desempenho na Bancada):** Rodando em modo Live CD direto de um pendrive com **Ventoy** (sem HD instalado na máquina e apenas com 4 GB de RAM), permaneceu **7 dias seguidos em funcionamento contínuo**, reproduzindo transmissão ao vivo no YouTube em 720p em tela cheia sem nenhum travamento.
* **Acer Aspire E1-510 e Samsung RV415:** Funcionam de forma estável para uso diário, mas suas arquiteturas mais antigas exigem evitar o acúmulo de muitas abas pesadas abertas simultaneamente.

### Teste Extremo de Múltiplos Monitores e Áudio HDMI
* O áudio e vídeo **HDMI** já vêm pré-configurados e funcionais na ISO.
* Em teste de estresse usando **3 telas simultâneas (tela interna do notebook + saída VGA + saída HDMI)**, o **Positivo S14BW01** suportou todas as telas ativas com estabilidade, enquanto os controladores gráficos mais antigos do Samsung RV415 e Acer E1-510 atingiram seu limite físico.
* *Dica:* Se conectar uma TV/monitor via HDMI e o som continuar no notebook, abra **Configurações > Som** no GNOME e selecione a saída **HDMI / DisplayPort**.

---

## 📥 4. Espelhos Oficiais de Download e Integridade

Baixe a imagem exclusivamente pelos repositórios oficiais abaixo e confira a assinatura SHA-256 antes de gravar no pendrive:

* **Internet Archive (Download Direto / Torrent):** [arch-gnome-legacy-elementista-x86_64](https://archive.org/details/arch-gnome-legacy-elementista-x86_64)
* **SourceForge (Espelho Global):** [arch-legacy-elementista](https://sourceforge.net/projects/arch-legacy-elementista/)
* **Blog e Documentação Complementar:** [Elementista Blog](https://elementista.blogspot.com/)
* **SHA-256 Oficial:** `8627c82802ee784a606d837a1602becf8df56d6272f6e5512406b400aca5309f`
---

## 🛠️ 5. Guia Pós-Instalação (Comandos Transparentes e Opcionais)

> **⚠️ AVISO SOBRE A LOJA GRÁFICA (GNOME SOFTWARE) VS. TERMINAL (`pacman`):**  
> A loja gráfica acompanha o sistema por fazer parte do ambiente padrão do GNOME, mas **não é o gerenciador oficial do Arch Linux** e instala majoritariamente pacotes **Flatpak**. Em computadores com **4 GB de RAM e HD mecânico**, pacotes Flatpak ocupam gigabytes de disco duplicados, consomem mais RAM e abrem mais devagar.  
> **Conheça os limites do seu hardware:** dê preferência total aos comandos nativos do terminal abaixo.

*(Nota: Ao digitar sua senha após um comando com `sudo`, não aparecem asteriscos na tela por segurança. Digite normalmente e aperte `Enter`.)*

### 1. Atualizar o Sistema com Segurança
Sempre antes de instalar qualquer programa, rode o script criado para esta ISO (ele renova o chaveiro do Arch Linux e atualiza os pacotes sem erros):
```bash
atualizar-arch
```

### 2. Pacotes Complementares Recomendados (Instale um por vez)
Para manter a ISO leve, alguns complementos ficaram de fora. Instale apenas o que precisar:

* **Codecs completos de áudio e vídeo (FFmpeg e formatos restritos):**
```bash
sudo pacman -S gst-libav gst-plugins-ugly
```

* **Aceleração de vídeo por hardware (VA-API — reduz o uso do processador em vídeos):**
```bash
sudo pacman -S gst-plugin-va
```

* **Suporte completo para arquivos compactados (`.rar`, `.zip`, `.7z`):**
```bash
sudo pacman -S p7zip unrar file-roller
```

### 3. Navegadores Web (Conforme a Sua Preferência)
A ISO já traz o Brave Browser instalado. Caso prefira outras opções:

* **Chromium (100% Código Aberto — Repositório Oficial do Arch):**
```bash
sudo pacman -S chromium
```

* **Firefox (Oficial em Português — Repositório Oficial do Arch):**
```bash
sudo pacman -S firefox firefox-i18n-pt-br
```

* **Google Chrome (Binário Oficial da Google — via AUR):**
```bash
yay -S google-chrome
```

* **Microsoft Edge (Binário Oficial da Microsoft — via AUR):**
```bash
yay -S microsoft-edge-stable-bin
```

#### Dica para Economizar Memória RAM (Atalhos Web para WhatsApp, Discord e YouTube)
Em vez de instalar aplicativos pesados que consomem muita RAM em segundo plano, transforme sites em janelas próprias usando o **Chromium** (ou Brave):
1. Abra o site desejado (ex: WhatsApp Web ou Discord) e faça login.
2. Clique nos **três pontinhos (`⋮`)** no canto superior direito.
3. Vá em **Salvar e compartilhar** (ou *Mais ferramentas*) > **Criar atalho...**
4. Dê um nome, marque a caixa **"Abrir como janela"** e clique em **Criar**.
5. Clique com o botão direito no ícone aberto na barra inferior e selecione **Fixar no painel**.
### 4. Outros Programas e Periféricos Opcionais

* **OBS Studio (Gravação de tela e transmissões — Oficial):**
```bash
sudo pacman -S obs-studio
```
*(Dica: No GNOME, adicione a fonte **"Captura de Tela (PipeWire)"**).*

* **LibreOffice em Português (Textos, planilhas e slides):**
```bash
sudo pacman -S libreoffice-fresh libreoffice-fresh-pt-br
```

* **Editor de texto e planilhas ultraleves (para hardware muito antigo):**
```bash
sudo pacman -S abiword gnumeric
```

* **Suporte a Impressoras e Scanners:**
```bash
sudo pacman -S cups system-config-printer
sudo systemctl enable --now cups
```

* **Suporte a Bluetooth:**
```bash
sudo pacman -S bluez bluez-utils
sudo systemctl enable --now bluetooth
```

* **Limpar arquivos temporários de instalação (liberar espaço em HD pequeno):**
```bash
sudo pacman -Sc
```

---

## 🔄 6. Configuração Opcional do GRUB (Dual Boot com Windows)

> **ATENÇÃO — CUIDADO AO EDITAR O BOOT:**  
> Altere apenas a linha indicada abaixo no arquivo `/etc/default/grub`. Nunca modifique `/boot/grub/grub.cfg` diretamente.

Por segurança, o Arch Linux vem com a detecção de outros sistemas operacionais desativada. Caso tenha instalado o sistema ao lado do Windows e ele não apareça no menu de boot:

1. **Instale o detector de sistemas (`os-prober`):**
```bash
sudo pacman -S os-prober
```

2. **Abra o arquivo de configuração principal com o `nano`:**
```bash
sudo nano /etc/default/grub
```

3. **Desça até o final do arquivo e altere a linha:**
De:
```text
GRUB_DISABLE_OS_PROBER=true
```
Para:
```text
GRUB_DISABLE_OS_PROBER=false
```

4. **Salve e feche o `nano`:**
* Aperte `Ctrl + O` e depois `Enter` para salvar.
* Aperte `Ctrl + X` para sair.

5. **Atualize o menu de boot:**
```bash
sudo grub-mkconfig -o /boot/grub/grub.cfg
```
*(Aguarde o terminal concluir e listar os sistemas encontrados antes de reiniciar).*

---

## ⌨️ Atalhos Rápidos do Ambiente GNOME

* **`Super` (tecla com a bandeira do Windows):** Abre a visão geral das janelas e permite digitar imediatamente o nome de qualquer programa ou arquivo para abrir.
* **`Super + A`:** Exibe a grade completa com todos os aplicativos instalados.

---

**Desenvolvido e validado por Elementista (Jorge).**

