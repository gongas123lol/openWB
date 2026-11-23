<div align="center">

# openWB

### Controlador wideband · Hardware baseado no SLC Free

Uma nova seleção de componentes e alterações de design, com o objetivo de tornar a utilização mais simples.

![Altium Designer](https://img.shields.io/badge/EDA-Altium_Designer-A5915F?style=for-the-badge)
![Hardware](https://img.shields.io/badge/Projeto-Hardware-2563EB?style=for-the-badge)
![Base](https://img.shields.io/badge/Base-SLC_Free-334155?style=for-the-badge)

[Sobre](#sobre-o-projeto) · [Design](#o-que-muda) · [Ficheiros](#ficheiros-do-projeto) · [Começar](#como-explorar-o-projeto)

</div>

---

## Sobre o projeto

O **openWB** é um projeto de hardware para um controlador de sonda lambda de banda larga (*wideband*), baseado no **SLC Free da 14Point7**. Este tipo de controlador permite acompanhar a mistura ar/combustível através da leitura de uma sonda de oxigénio.

O projeto parte do design SLC Free e adapta-o com **uma escolha diferente de componentes e alterações ao design da placa**, tendo como objetivo facilitar a sua utilização e integração. Este repositório reúne o esquema elétrico, o layout da PCB e os ficheiros de saída do projeto em Altium Designer.

## O que muda

| Área | Abordagem do openWB |
| --- | --- |
| **Componentes** | Seleção de componentes diferente da utilizada no design de referência. |
| **Design da placa** | Alterações ao circuito e à implementação física para acomodar as opções do projeto. |
| **Utilização** | Adaptações orientadas para tornar o hardware mais prático de utilizar e integrar. |
| **Ficheiros de projeto** | Esquema e PCB editáveis em Altium Designer, acompanhados de outputs de fabrico e montagem. |

Os ficheiros de esquema, PCB e lista de materiais documentam a implementação concreta do openWB. A compatibilidade elétrica, a pinagem e os requisitos de firmware devem ser confirmados para esta implementação; a origem no SLC Free, por si só, não confirma que sejam iguais aos do projeto original.

## Ficheiros do projeto

| Ficheiro / pasta | Conteúdo |
| --- | --- |
| [`openWB.PrjPcb`](openWB.PrjPcb) | Projeto principal do Altium Designer. |
| [`main.SchDoc`](main.SchDoc) | Esquema elétrico. |
| [`main.PcbDoc`](main.PcbDoc) | Layout e encaminhamento da PCB. |
| [`Job3.OutJob`](Job3.OutJob) | Configuração de geração de outputs. |
| [`Project Outputs for openWB/`](Project%20Outputs%20for%20openWB/) | Gerbers, ficheiros de furação, pick and place e outros outputs exportados. |
| [`Project Outputs for Free Documents/`](Project%20Outputs%20for%20Free%20Documents/) | Lista de materiais e ficheiros adicionais de montagem. |
| [`History/`](History/) | Histórico local de versões guardadas pelo Altium. |
| [`Project Logs for openWB/`](Project%20Logs%20for%20openWB/) | Registos de alterações gerados pelo Altium. |

### Acesso rápido

- **[Lista de materiais — BOM](Project%20Outputs%20for%20Free%20Documents/BOM/Bill%20of%20Materials-openWB.xlsx)**
- **[Pick and place — CSV](Project%20Outputs%20for%20openWB/Pick%20Place/Pick%20Place%20for%20main.csv)**
- **[Documento exportado — PDF](Project%20Outputs%20for%20openWB/Job4.PDF)**

## Como explorar o projeto

1. Clona o repositório:

   ```bash
   git clone https://github.com/gongas123lol/openWB.git
   ```

2. Abre `openWB.PrjPcb` no **Altium Designer**.
3. Consulta `main.SchDoc` para compreender o circuito e as ligações.
4. Abre `main.PcbDoc` para explorar a disposição dos componentes e o layout.
5. Consulta a BOM e os outputs para analisar os componentes e a montagem.

Se alterares o projeto, volta a gerar os outputs a partir da revisão que pretendes fabricar. Os ficheiros exportados incluídos no repositório são snapshots e podem não refletir alterações posteriores ao esquema ou à PCB.

## Documentação técnica

Este repositório centra-se no **hardware**. Não inclui atualmente código-fonte de firmware nem um guia completo de configuração, calibração e ligação. O esquema e a BOM são o ponto de partida para verificar os componentes, conectores e ligações desta versão.

## Origem e créditos

O openWB baseia-se no **SLC Free**, desenvolvido pela **14Point7**. O crédito pelo design de referência pertence aos seus autores; o openWB reúne as escolhas de componentes e adaptações de design deste projeto.

- [14Point7 — documentação, código-fonte e ficheiros do SLC Free](https://www.14point7.com/pages/software-and-documentation)
- [Repositório openWB](https://github.com/gongas123lol/openWB)

---

<div align="center">

**openWB** · Do design de referência a uma implementação adaptada.

</div>
