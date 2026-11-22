# Resposta a incidente em um site WordPress (estudo de caso)

**Português** · [English](README.en.md)

Investigação e limpeza de um site institucional em WordPress que estava comprometido havia cerca de 11 meses sem que ninguém percebesse. O site servia páginas de golpe ao Google e roubava dados digitados em formulários. Tudo foi resolvido no mesmo dia, sem tirar o site do ar.

Feito durante meu trabalho em uma consultoria ambiental. Apenas documentação: sem domínios, nomes de arquivos maliciosos, credenciais ou identificadores de infraestrutura.

> O site e a hospedagem pertencem ao empregador. Este repositório descreve o método e as decisões, não o ambiente. Veja [O que foi omitido intencionalmente](#o-que-foi-omitido-intencionalmente).

## Como começou

Em novembro de 2026, o gerente avisou que, ao buscar o nome da empresa no Google, o primeiro resultado aparecia com outro nome: uma loja falsa em alemão. Quem digitava o endereço no navegador via o site normal, então ninguém tinha estranhado.

## Meu papel

Fiz sozinho a investigação, a contenção, a limpeza e o acompanhamento, com acesso ao painel da hospedagem (gerenciador de arquivos, banco de dados e logs) e ao WordPress. Usei desenvolvimento assistido por IA (Claude Code) para ler os arquivos e cruzar as datas. As decisões sobre o que remover e quando foram minhas.

## Investigação

- **Porta de entrada:** uma backdoor (um gerenciador de arquivos disfarçado) escondida dentro de um tema. Pela data dos arquivos, ela estava lá desde **25/12/2025**, ou seja, o site ficou comprometido por cerca de 11 meses.
- **Cloaking:** kits de golpe publicados em subpastas respondiam de forma diferente conforme o visitante. O robô do Google e quem chegava por anúncios viam a loja falsa; quem acessava direto via o site real. Por isso o problema não aparecia para ninguém da empresa. Os anúncios estavam ligados a uma operação de golpe no exterior.
- **Roubo de dados:** um carregador escondido no núcleo do WordPress consultava um servidor externo e **enviava para fora o que era digitado nos formulários** do site.
- **Reinfecção:** um segundo arquivo de instalação tinha sido plantado em 31/10/2026, sinal de que o acesso ainda estava ativo.

O método foi reconstruir a linha do tempo pelas datas de modificação dos arquivos, comparar o núcleo do WordPress com a versão oficial e conferir cada plugin e tema: está em uso? foi alterado? de onde veio?

## Contenção e limpeza

- **Quarentena em vez de apagar.** Tudo o que estava comprometido ou sem uso foi movido para uma pasta fora da área pública: 22 plugins, 8 temas, subdomínios antigos e um `wp-config` antigo. O site continuou funcionando sem eles, e as evidências ficaram guardadas.
- **Troca de todos os segredos.** Senha do banco, chaves e salts do WordPress e senhas dos administradores.
- **Endurecimento:** edição de arquivos pelo painel desativada, `xmlrpc` bloqueado, execução de PHP bloqueada na pasta de uploads e atualizações automáticas ativadas.
- **Reputação no Google:** verifiquei o domínio no Google Search Console (por DNS), pedi a revisão do alerta de "páginas enganosas" e solicitei a reindexação.
- **Monitoramento:** criei uma verificação semanal (toda segunda às 09:00) que compara os arquivos do site com uma linha de base limpa e avisa se algo mudar.

## Resultado

- Site limpo no mesmo dia, **sem tempo fora do ar**.
- Vazamento de dados dos formulários interrompido; acesso do invasor encerrado com a troca dos segredos.
- Revisão do Google solicitada e monitoramento contínuo ativo.

## Decisões

- **Não tirar o site do ar.** Como o site real funcionava e o golpe estava em arquivos separados, deu para limpar com o site no ar, sem prejudicar quem acessava.
- **Quarentena antes de apagar.** Guardar permite investigar depois e desfazer uma remoção errada. A exclusão definitiva ficou marcada para dezembro de 2026, depois de um período sem reinfecção.
- **Supor que tudo vazou.** Como havia um carregador ativo, todos os segredos foram trocados, não só os que pareciam expostos.

## Limitações

- Não foi possível identificar com certeza qual plugin trouxe o código malicioso. O mais provável é um dos que estão em quarentena.
- A data de entrada vem das datas dos arquivos, que um invasor pode alterar. É uma estimativa, não uma prova.

## Lições

- Um site sem manutenção vira risco de reputação e ponto de vazamento de dados, mesmo sendo "só o site institucional".
- Cloaking esconde o problema justamente de quem é dono do site. Vale buscar o nome da empresa no Google de vez em quando, e não só abrir o endereço.
- Uma rotina simples resolve a maior parte: atualizações automáticas, remover o que não é usado, revisar quem tem acesso e monitorar mudanças nos arquivos.

## O que foi omitido intencionalmente

- **Domínio, hospedagem e nomes de arquivos.** Não têm valor para portfólio e só ajudariam quem quisesse atacar de novo.
- **Indicadores de comprometimento** (endereços de servidores externos, nomes e hashes dos arquivos maliciosos).
- **Credenciais e configurações**, antigas ou novas.

---

> © Henry Noguchi. **Todos os direitos reservados.** Repositório público só para leitura; copiar, reutilizar ou apresentar como próprio exige autorização. Ver [LICENSE](LICENSE).
