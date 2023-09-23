# Teoria-de-la-informaci-n

Fuente de información: 

En una biblioteca necesitan subir  libros digitales a su galería web para ello necesitan comunicarse con su servidor.

Transmisor:

Para ello necesita transformar los libros en paquetes binarios para un mejor rendimiento y velocidad, por lo tanto el código necesita transformar el contenido de cada libro en datos binarios y dividirlo en cierto tamaño para encapsulación.

Canal:

El código simulará un canal en el cual se pasará los paquetes, a cierta velocidad para ser enviado al receptor, también se le agrega ruido de modificación de los paquetes.

Receptor:

El receptor recibe los paquetes en binario y se va a encargar de desencapsular los paquetes, para después transformar los paquetes a su estado original al momento de ser enviado.

Destino de información:

Una vez teniendo los datos originales, estos se subirán a la base de datos de la galería web.



Mi codigo lo cree con ayuda de chat gpt y de ahi lo fui modificando usando igual chat gpt, esto fue lo que me entrego pasando los parametros del read y explicando que el canal era de wifi y luego le dije que sea un poco mas realista y cambio los parametros del ruido al igual le dije que introduciera la funcion para calcular la entropia y el resultado se puede ver en  **Medios_de_comunicacion_cambios**: 




# Codigo
 > #Generador por cht gpt
>  import random
>  import base64
>  import re

>  def encode_book(file_path):
>     try:
>         with open(file_path, "rb") as archivo:
>            file_content = archivo.read()
>             encoded_content = base64.b64encode(file_content)
>        packet_size = 1024  # Tamaño del paquete en bytes
>        packets = [encoded_content[i:i + packet_size] for i in range(0, len(encoded_content), packet_size)]
>        return packets
>    except FileNotFoundError:
>        print(f"El archivo '{file_path}' no se encontró.")
>        return []
>  def apply_wifi_noise(packet, reverse=False):
>    if reverse:
>        cleaned_packet = packet[::-1]
>        return cleaned_packet
>    else:
>        if random.random() < 0.1:
>            return None
>        if random.random() < 0.05:
>            return packet[::-1]
>        return packet
>  def simulate_wifi_channel(packets):
>    noisy_packets = []
>    for packet in packets:
>        noisy_packet = apply_wifi_noise(packet, reverse=False)
>        if noisy_packet:
>            noisy_packets.append(noisy_packet)
>    return noisy_packets
>  def decode_packets(noisy_packets):
>    cleaned_packets = []
>    for noisy_packet in noisy_packets:
>        cleaned_packet = apply_wifi_noise(noisy_packet, reverse=True)
>        if cleaned_packet:
>            cleaned_packets.append(cleaned_packet)
>    return cleaned_packets
>  def reconstruct_book(cleaned_packets):
>    combined_data = b''.join(cleaned_packets)
>    # Eliminar cualquier caracter que no sea válido en base64
>    combined_data = re.sub(b'[^a-zA-Z0-9/+]', b'', combined_data)
>    # Agregar padding válido si es necesario
>    padding_length = len(combined_data) % 4
>    if padding_length > 0:
>        combined_data += b'=' * (4 - padding_length)
>    book_content = base64.b64decode(combined_data)
>    return book_content
>    # Nombre del archivo de entrada
>  input_file_path = "QUIMICA 2 INVESTIGACION.pdf"
>    # Codificar el libro
>  book = encode_book(input_file_path)
>    # Simular la transmisión a través de un canal WiFi
>  book_env = simulate_wifi_channel(book)
>    # Decodificar los paquetes recibidos
>  book_decode = decode_packets(book_env)
>    # Reconstruir el libro
>  book_reconst = reconstruct_book(book_decode)
>    # Nombre del archivo de salida
>  output_file_path = "QUIMICA 2 INVESTIGACION2.pdf"
>    # Guardar el libro reconstruido en un nuevo archivo
>  with open(output_file_path, 'wb') as f:
>    f.write(book_reconst)
