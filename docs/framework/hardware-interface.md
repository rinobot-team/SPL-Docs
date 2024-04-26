# Interface com Hardware
*Documentação detalhada ainda não fornecida pela HULKs*

## **TODO**: Elaborar
- Interface de Hardware
    - `trait`
        - `produce_sensor_data()`
    - NAO
        - Aquisição de HardwareId (do HULA)
        - LoLA/HAL/HULA
            - Explicar siglas
            - Resumo: Estado/Conexão/Rede/Diagrama de Componentes, Modelo de DataFlow
            - Localização do socket e explicação do UnixSocket
            - Proxy
                - Extração e injetor de mensagens
                - Formatação de mensagens
                - Animações dos LEDs
            - Aliveness
                - Rede: Formato de mensagem, UDP, multicast, JSON
                - Estados de serviço
            - `produce_sensor_data()`
        - Cameras
            - Video4Linux
            - Buffering, Zero-copy (-> Cycler)
            - Configuração de câmeras (registers)
        - Audio
            - ALSA
            - Configuração do ALSA
            - Depois: Audio playback, Text-to-speech
    - Webots
        - Aquisição de HardwareId (robot name)
        - Webots bindings
        - `produce_sensor_data()`
        - Transferência de áudio e imagem para diferentes threads
        - Simulation World
        - Estrutura de diretórios e symlink