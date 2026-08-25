# Criando links com ln: hardlinks e softlinks

## O nome de um arquivo não é o arquivo

Para entender links no Linux, é preciso primeiro desfazer uma suposição comum: o nome de um arquivo, aquele texto que aparece quando se roda `ls`, não é o arquivo em si. Por trás de cada nome existe uma estrutura chamada inode, que guarda os dados de verdade sobre aquele arquivo, permissões, dono, tamanho, data de modificação, e a localização real do conteúdo no disco. O nome do arquivo é só uma entrada num diretório que aponta para um inode específico. Essa separação entre nome e conteúdo é justamente o que torna possível ter mais de um nome apontando para os mesmos dados, que é o que um link faz.

O comando usado para criar qualquer tipo de link é o `ln`.

## Hardlink: outro nome para o mesmo inode

Um hardlink cria um novo nome que aponta diretamente para o mesmo inode do arquivo original, sem criar um arquivo novo de verdade.

```
ln arquivo_original.txt hardlink.txt
```

Depois desse comando, `arquivo_original.txt` e `hardlink.txt` são, para todos os efeitos práticos, o mesmo arquivo, só que acessível por dois nomes diferentes. Alterar o conteúdo através de um dos nomes altera o conteúdo visto pelo outro também, porque os dois nomes apontam exatamente para o mesmo inode, com os mesmos dados.

Uma consequência interessante é que apagar `arquivo_original.txt` não apaga os dados de verdade, só remove esse nome específico. Como `hardlink.txt` ainda aponta para o mesmo inode, o conteúdo continua acessível através dele. Os dados só desaparecem de fato quando o último nome que aponta para aquele inode é removido.

Hardlinks têm duas limitações importantes: só funcionam dentro do mesmo sistema de arquivos (não é possível criar um hardlink apontando para um arquivo que está num disco diferente), e não podem ser usados em pastas, só em arquivos comuns.

## Softlink (link simbólico): um atalho de verdade

Um softlink, ou link simbólico, funciona de um jeito bem mais parecido com o atalho tradicional do Windows: em vez de apontar para o mesmo inode do arquivo original, ele é, na verdade, um arquivo próprio, pequeno, cujo conteúdo é apenas o caminho até o arquivo de destino.

```
ln -s arquivo_original.txt softlink.txt
```

A diferença de comportamento em relação ao hardlink é significativa. Se o arquivo original for apagado ou movido, o softlink continua existindo, mas passa a apontar para um caminho que não existe mais, o que é chamado de link quebrado, e tentar abri-lo resulta em erro. Em compensação, softlinks não têm as limitações dos hardlinks: podem apontar para arquivos em discos diferentes, e podem apontar também para pastas inteiras, não só para arquivos individuais.

## Escolhendo entre os dois

Softlinks são a escolha mais comum no dia a dia, justamente pela flexibilidade, sendo usados com frequência para criar atalhos de acesso rápido, ou para manter compatibilidade quando um programa espera encontrar um arquivo num caminho específico, mas o arquivo de verdade está guardado em outro lugar. Hardlinks são mais usados em cenários mais específicos, como sistemas de backup que precisam economizar espaço em disco mantendo várias versões de um mesmo arquivo sem duplicar o conteúdo de fato, contanto que ele não tenha mudado entre uma versão e outra.

## Fontes

- [Hard Links vs Symbolic Links in Linux, Linuxize](https://linuxize.com/post/hard-links-vs-symbolic-links/)
- [Symbolic vs. Hard Links in Linux: What You Need to Know, How-To Geek](https://www.howtogeek.com/symbolic-vs-hard-links-in-linux/)
- [ln(1), Linux manual page, man7.org](https://man7.org/linux/man-pages/man1/ln.1.html)
