# Campo.Minado.Victor
#printa campo visível
def campovisivel(campov):
  c = 0
  for i in range(0, 6):
    print(campov[c], campov[c+1], campov[c+2], campov[c+3], campov[c+4], campov[c+5])
    c = c + 6

from random import randrange
#Torno = posições que ficam em torno do número em uma matriz 6x6
torno = [-7, -6, -5, -1, 1, 5, 6, 7]

#Gerar campo
campo = []
for i in range(36):
  campo.append(0)

#Gerar campo visível
campov = []
for i in range(36):
  campov.append('#')

#adiciona bombas
for i in range(0, 7):
#Evita que uma bomba nasça em cima da outra
  while(True):
    r = randrange(36)
    if(campo[r] != 9):
      break
  campo[r] = 9

#adicionar números:
#O número pode dar errado por 3 motivos: I - Ele está em um campo inexistente; II Ele está "em torno" da bomba, porém do outro lado do campo(quando a bomba
#está em um canto); III Existia uma bomba nessa posição.
  j = -7
  for j in torno:
#evita I
    if(r + j < 36 and r + j > -1):
#Evita II{r % 6 = 0(canto esquerdo), r % 6 = 5(canto direito) quando ela está no direito r-7, r-1 e r+5 não a tocam, e no esquerdo r-5, r+1, r+7}
      if(r % 6 == 0 and j != -7 and j != -1 and j != 5 or r % 6 == 5 and j != -5 and j != 1 and j != 7 or r % 6 != 0 and r % 6 != 5):
#Evita III
        if(campo[r+j] != 9):
          campo[r+j] = campo[r+j] + 1

desc = int(0)
playing = True
while(playing):
  posicaoc = int(99)
  posicaol = int(99)
  campovisivel(campov)
  while(posicaol > 6 or posicaol < 1):
    posicaol = int(input('linha: '))
  while(posicaoc > 6 or posicaoc < 1):
    posicaoc = int(input('coluna: '))
  posicao = (posicaol - 1) * 6 + posicaoc - 1
  if(campov[posicao] == '#'):
#evita dar ponto com número que ja foi descoperto
    if(campov[posicao] == '#'):
      desc = desc + 1
    campov[posicao] = campo[posicao]
#revela o entorno
    j = -7
    for j in torno:
      if(posicao + j < 36 and posicao + j > -1):
        if(posicao % 6 == 0 and j != -7 and j != -1 and j != 5 or posicao % 6 == 5 and j != -5 and j != 1 and j != 7 or posicao % 6 != 0 and posicao % 6 != 5):
          if(campo[posicao+j] != 9):
            if(campov[posicao+j] == '#'):
              desc = desc + 1
            campov[posicao+j] = campo[posicao+j]
    if(campov[posicao] == 9):
      print('Você perdeu :(')
      playing = False
    if(desc >= 28):
      print('Você ganhouu!!')
      playing = False
