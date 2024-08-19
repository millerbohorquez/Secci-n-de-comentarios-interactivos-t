<script>
  import { onMount } from "svelte";  

  let data = {
    currentUser: {
      image: {
        png: "./images/avatars/image-juliusomo.png",
        webp: "./images/avatars/image-juliusomo.webp",
      },
      username: "juliusomo",
    },
    comments: [
      {
        parent: 0,
        id: 1,
        content:
          "Impressive! Though it seems the drag feature could be improved. But overall it looks incredible. You've nailed the design and the responsiveness at various breakpoints works really well.",
        createdAt: "1 month ago",
        score: 12,
        user: {
          image: {
            png: "./images/avatars/image-amyrobson.png",
            webp: "./images/avatars/image-amyrobson.webp",
          },
          username: "amyrobson",
        },
        replies: [],
      },
      {
        parent: 0,
        id: 2,
        content:
          "Woah, your project looks awesome! How long have you been coding for? I'm still new, but think I want to dive into React as well soon. Perhaps you can give me an insight on where I can learn React? Thanks!",
        createdAt: "2 weeks ago",
        score: 5,
        user: {
          image: {
            png: "./images/avatars/image-maxblagun.png",
            webp: "./images/avatars/image-maxblagun.webp",
          },
          username: "maxblagun",
        },
        replies: [
          {
            parent: 2,
            id: 1,
            content:
              "If you're still new, I'd recommend focusing on the fundamentals of HTML, CSS, and JS before considering React. It's very tempting to jump ahead but lay a solid foundation first.",
            createdAt: "1 week ago",
            score: 4,
            replyingTo: "maxblagun",
            user: {
              image: {
                png: "./images/avatars/image-ramsesmiron.png",
                webp: "./images/avatars/image-ramsesmiron.webp",
              },
              username: "ramsesmiron",
            },
          },
          {
            parent: 2,
            id: 2,
            content:
              "I couldn't agree more with this. Everything moves so fast and it always seems like everyone knows the newest library/framework. But the fundamentals are what stay constant.",
            createdAt: "2 days ago",
            score: 2,
            replyingTo: "ramsesmiron",
            user: {
              image: {
                png: "image/",
                webp: "images/avatars/image-juliusomo.webp",
              },
              username: "juliusomo",
            },
          },
        ],
      },
    ],
  };

  let newComment = ''; 

function addComment(body, parentId = 0, replyTo = undefined) {
  let commentParent =
    parentId === 0
      ? data.comments
      : data.comments.find((c) => c.id == parentId).replies;
  let newComment = {
    parent: parentId,
    id: commentParent.length == 0 ? 1 : commentParent[commentParent.length - 1].id + 1,
    content: body,
    createdAt: "Now",
    replyingTo: replyTo,
    score: 0,
    replies: parentId == 0 ? [] : undefined,
    user: data.currentUser,
  };
  commentParent.push(newComment);
  data = { ...data };
}

function deleteComment(commentObject) {
  if (commentObject.parent == 0) {
    data.comments = data.comments.filter((e) => e != commentObject);
  } else {
    let parentComment = data.comments.find((e) => e.id === commentObject.parent);
    parentComment.replies = parentComment.replies.filter((e) => e != commentObject);
  }
  data = { ...data };
}

function promptDel(commentObject) {
  let modalWrp = document.querySelector(".modal-wrp");
  modalWrp.classList.remove("invisible");
  modalWrp.querySelector(".yes").addEventListener("click", () => {
    deleteComment(commentObject);
    modalWrp.classList.add("invisible");
  });
  modalWrp.querySelector(".no").addEventListener("click", () => {
    modalWrp.classList.add("invisible");
  });
}

function spawnReplyInput(parentId, replyTo = undefined) {
  let commentBody = prompt("Enter your reply:");
  if (commentBody.length > 0) {
    addComment(commentBody, parentId, replyTo);
  }
}


function scorePlus(commentObject) {
  commentObject.score++;
  data = { ...data };
}

function scoreMinus(commentObject) {
  if (commentObject.score > 0) {
    commentObject.score--;
    data = { ...data };
  }
}
</script>


<main class="container">
  <div class="comment-section ">
    <div class="comments-wrp ">
      {#each data.comments as comment}
        <div class="comment-wrp ">
          <div class="comment">
            <div class="c-score">
              <svg class="score-control" on:click={() => scorePlus(comment)} width="11" height="11" xmlns="http://www.w3.org/2000/svg"><path d="M6.33 10.896c.137 0 .255-.05.354-.149.1-.1.149-.217.149-.354V7.004h3.315c.136 0 .254-.05.354-.149.099-.1.148-.217.148-.354V5.272a.483.483 0 0 0-.148-.354.483.483 0 0 0-.354-.149H6.833V1.4a.483.483 0 0 0-.149-.354.483.483 0 0 0-.354-.149H4.915a.483.483 0 0 0-.354.149c-.1.1-.149.217-.149.354v3.37H1.08a.483.483 0 0 0-.354.15c-.1.099-.149.217-.149.353v1.23c0 .136.05.254.149.353.1.1.217.149.354.149h3.333v3.39c0 .136.05.254.15.353.098.1.216.149.353.149H6.33Z" fill="#C5C6EF"/></svg>
              <p class="score-number font-medium">{comment.score}</p>
              <svg class="score-control" on:click={() => scoreMinus(comment)} width="11" height="3" xmlns="http://www.w3.org/2000/svg"><path d="M9.256 2.66c.204 0 .38-.056.53-.167.148-.11.222-.243.222-.396V.722c0-.152-.074-.284-.223-.395a.859.859 0 0 0-.53-.167H.76a.859.859 0 0 0-.53.167C.083.437.009.57.009.722v1.375c0 .153.074.285.223.396a.859.859 0 0 0 .53.167h8.495Z" fill="#C5C6EF"/></svg>
            </div>
            <div class="c-user">
              <img src={comment.user.image.webp} alt="" class="usr-img">
              <p class="usr-name">{comment.user.username}</p>
              <p class="cmnt-at">{comment.createdAt}</p>
            </div>
            <div class="c-controls">
              {#if comment.user.username == data.currentUser.username}
              <a class="c-delete " on:click={() => promptDel(comment)}>
                <svg width="12" height="14" xmlns="http://www.w3.org/2000/svg"><path d="M1.167 12.448c0 .854.7 1.552 1.555 1.552h6.222c.856 0 1.556-.698 1.556-1.552V3.5H1.167v8.948Zm10.5-11.281H8.75L7.773 0h-3.88l-.976 1.167H0v1.166h11.667V1.167Z" fill="#ED6368"/></svg>                <span>Delete</span>
              </a>
                <a class="c-edit">
                  <svg width="14" height="14" xmlns="http://www.w3.org/2000/svg"><path d="M13.479 2.872 11.08.474a1.75 1.75 0 0 0-2.327-.06L.879 8.287a1.75 1.75 0 0 0-.5 1.06l-.375 3.648a.875.875 0 0 0 .875.954h.078l3.65-.333c.399-.04.773-.216 1.058-.499l7.875-7.875a1.68 1.68 0 0 0-.061-2.371Zm-2.975 2.923L8.159 3.449 9.865 1.7l2.389 2.39-1.75 1.706Z" fill="#5357B6"/></svg>
                  <span>Edit</span>
                </a>
              {/if}
              <a class="c-reply" on:click={() => spawnReplyInput(comment.id)}>
                <svg width="14" height="13" xmlns="http://www.w3.org/2000/svg"><path d="M.227 4.316 5.04.16a.657.657 0 0 1 1.085.497v2.189c4.392.05 7.875.93 7.875 5.093 0 1.68-1.082 3.344-2.279 4.214-.373.272-.905-.07-.767-.51 1.24-3.964-.588-5.017-4.829-5.078v2.404c0 .566-.664.86-1.085.496L.227 5.31a.657.657 0 0 1 0-.993Z" fill="#5357B6"/></svg>
                <span>Reply</span>
              </a>
            </div>
            <p class="c-text ">
              {#if comment.replyingTo}
                <span class="reply-to">@{comment.replyingTo}</span>
              {/if}
              <span class="c-body">{comment.content}</span>
            </p>
          </div>
          {#if comment.replies.length > 0}
            <div class="replies comments-wrp ">
              {#each comment.replies as reply}
                <div class="comment">
                  <!-- Repite la estructura del comentario aquí para las respuestas -->
                  <div class="c-user">
                    <img src={reply.user.image.webp} alt="" class="usr-img">
                    <p class="usr-name">{reply.user.username}</p>
                    <p class="cmnt-at">{reply.createdAt}</p>
                  </div>
                  <p class="c-text">
                    {#if reply.replyingTo}
                      <span class="reply-to">@{reply.replyingTo}</span>
                    {/if}
                    <span class="c-body">{reply.content}</span>
                  </p>
                </div>
              {/each}
            </div>
          {/if}
        </div>
      {/each}
    </div>

    <div class="reply-input">
      <img src={data.currentUser.image.webp} alt="" class="usr-img">
      <textarea class="cmnt-input" placeholder="Add a comment..." bind:value={newComment}></textarea>
      <button class="bu-primary" on:click={() => {
        if (newComment.length > 0) {
          addComment(newComment);
          newComment = '';
        }
      }}>SEND</button>
    </div>
  </div>

  <div class="modal-wrp invisible">
    <div class="modal container">
      <h3 class="">Delete comment</h3>
      <p class="">Are you sure you want to delete this comment? This will remove the comment and can't be undone.</p>
      <div class="flex justify-between">
        <button class="no">NO, CANCEL</button>
        <button class="yes">YES, DELETE</button>
      </div>
    </div>
  </div>
</main>