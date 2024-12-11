


pygame.init()
largura, altura = 800, 600
tela = pygame.display.set_mode((largura, altura))
pygame.display.set_caption("Jogo de Plataforma")
relogio = pygame.time.Clock()

# Cores
BRANCO = (255, 255, 255)
AZUL = (0, 0, 255)

# Jogador
jogador = pygame.Rect(100, 500, 50, 50)
velocidade_jogador = 5
gravidade = 1
forca_pulo = -15
velocidade_y = 0
no_chao = False

# Plataforma
plataforma = pygame.Rect(0, 550, 800, 50)

# Moeda
moeda = pygame.Rect(400, 500, 30, 30)
moedas_coletadas = 0

# Função para desenhar tudo
def desenhar():
    tela.fill(BRANCO)
    pygame.draw.rect(tela, AZUL, jogador)
    pygame.draw.rect(tela, (0, 255, 0), plataforma)
    pygame.draw.rect(tela, (255, 215, 0), moeda)
    pygame.display.flip()

# Loop principal do jogo
while True:
    for evento in pygame.event.get():
        if evento.type == pygame.QUIT:
            pygame.quit()
            sys.exit()

    teclas = pygame.key.get_pressed()
    if teclas[pygame.K_LEFT]:
        jogador.x -= velocidade_jogador
    if teclas[pygame.K_RIGHT]:
        jogador.x += velocidade_jogador
    if teclas[pygame.K_SPACE] and no_chao:
        velocidade_y = forca_pulo

    # Aplicando gravidade
    velocidade_y += gravidade
    jogador.y += velocidade_y

    # Verificação de colisão com a plataforma
    if jogador.colliderect(plataforma):
        jogador.y = plataforma.y - jogador.height
        velocidade_y = 0
        no_chao = True
    else:
        no_chao = False

    # Colisão com moeda
    if jogador.colliderect(moeda):
        moeda.x, moeda.y = -100, -100  # Remove a moeda
        moedas_coletadas += 1
        print(f'Moedas coletadas: {moedas_coletadas}')

    desenhar()
    relogio.tick(30)
