# Media API Reference

This section covers all API methods related to sending, editing, and managing media files in TBL.

## Media Methods

| Method | Description |
|--------|-------------|
| [sendPhoto](send-photo.md) | Send a photo |
| [sendVideo](send-video.md) | Send a video |
| [sendAudio](send-audio.md) | Send an audio file |
| [sendDocument](send-document.md) | Send a file |
| [sendVoice](send-voice.md) | Send a voice message |
| [sendAnimation](send-animation.md) | Send an animation (GIF) |
| [sendVideoNote](send-video-note.md) | Send a video note |
| [sendMediaGroup](send-media-group.md) | Send a group of photos or videos |
| [editMessageMedia](edit-message-media.md) | Edit a media message |

## Basic Usage

```javascript
// Send a photo by URL
Api.sendPhoto({
  chat_id: chat.id,
  photo: "https://example.com/image.jpg",
  caption: "Beautiful image"
});

// Send a photo by file_id (from previously uploaded photo)
Api.sendPhoto({
  chat_id: chat.id,
  photo: "AgACAgIAAxkBAAECu7ZhF8hzCy-vZf9X...",
  caption: "Same photo again"
});

// Send a document with custom filename
Api.sendDocument({
  chat_id: chat.id,
  document: "https://example.com/file.pdf",
  caption: "Important document",
  filename: "important.pdf"
});
```

## Media with Formatting

```javascript
// Send photo with formatted caption
Api.sendPhoto({
  chat_id: chat.id,
  photo: "https://example.com/image.jpg",
  caption: "*Bold caption* with _italic_ text",
  parse_mode: "Markdown",
  has_spoiler: true  // Mark media as spoiler
});

// Send video with HTML caption
Api.sendVideo({
  chat_id: chat.id,
  video: "https://example.com/video.mp4",
  caption: "<b>Video title</b>\n<i>Description goes here</i>",
  parse_mode: "HTML",
  supports_streaming: true
});
```

## Media Groups

```javascript
// Send a group of photos
Api.sendMediaGroup({
  chat_id: chat.id,
  media: [
    {
      type: "photo",
      media: "https://example.com/image1.jpg",
      caption: "First image"
    },
    {
      type: "photo",
      media: "https://example.com/image2.jpg"
    },
    {
      type: "video",
      media: "https://example.com/video.mp4",
      caption: "A video in the group"
    }
  ]
});
```

## Editing Media

```javascript
// Edit a media message
Api.editMessageMedia({
  chat_id: chat.id,
  message_id: msg.message_id,
  media: {
    type: "photo",
    media: "https://example.com/new-image.jpg",
    caption: "Updated caption"
  }
});
```

## Related Topics

- [InputFile](../../types/input-file.md)
- [InputMedia Types](../../types/input-media.md)
- [Message Object](../../types/message.md)
- [File Handling](../../file-handling.md) 