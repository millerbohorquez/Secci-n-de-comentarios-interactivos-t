# Comentarios Interactivos

Este proyecto se basa en proporciona una funcionalidad completa para agregar, eliminar, editar y puntuar comentarios en una aplicación web. También incluye la capacidad de responder a comentarios, gestionar las respuestas y confirmar acciones realizadas.

## Tabla de Contenidos

- [Instalación](#instalación)
- [Uso](#uso)
- [Funciones Disponibles](#funciones-disponibles)
  - [Agregar Comentario](#agregar-comentario)
  - [Eliminar Comentario](#eliminar-comentario)
  - [Responder a Comentario](#responder-a-comentario)
  - [Editar Comentario](#editar-comentario)
  - [Puntuación de Comentarios](#puntuación-de-comentarios)
- [Manejo del Estado de Edición](#manejo-del-estado-de-edición)
- [Confirmación de Eliminación](#confirmación-de-eliminación)



# Uso
El script principal proporciona varias funciones que permiten al usuario agregar, eliminar, editar y puntuar comentarios. Puedes integrar estas funciones en tu aplicación web y personalizar su comportamiento según tus necesidades.

# Funciones Disponibles
aca se raliza una explicacion de las funciones corespondientes en el funcionamiento.

## Agregar Comentario

La función addComment(body, parentId = 0, replyTo = undefined) permite agregar un nuevo comentario. Si el comentario es una respuesta, parentId y replyTo deben ser especificados.

`addComment` ('Este es un nuevo comentario', 0);

## Eliminar Comentario

La función deleteComment(commentObject) elimina un comentario, ya sea un comentario principal o una respuesta.

`deleteComment(commentObject);`

## Responder a Comentario

La función addReply(newReplyContent) permite agregar una respuesta a un comentario existente.

`spawnReplyInput(commentId);`  
`addReply` ('Esta es una respuesta a un comentario existente');

## Editar Comentario
Para editar un comentario o respuesta, se utilizan dos funciones: editComment(comment, type) y updateComment().

`editComment(commentObject, 'comment');` 
`editComment(replyObject, 'reply');`      
`updateComment();`  

## Puntuación de Comentarios
Las funciones scorePlus(commentObject) y scoreMinus(commentObject) permiten aumentar o disminuir la puntuación de un comentario.

`scorePlus(commentObject);`
`scoreMinus(commentObject);`  

## Manejo del Estado de Edición
El estado de edición se maneja con las variables editingCommentId, editingCommentType y editContent. La función resetEditingState() restablece estas variables una vez que se ha actualizado el comentario.

`resetEditingState();`

## Confirmación de Eliminación
La función promptDel(commentObject) muestra un modal de confirmación antes de eliminar un comentario. Si el usuario confirma, se llama a deleteComment(commentObject).

`promptDel(commentObject); `

