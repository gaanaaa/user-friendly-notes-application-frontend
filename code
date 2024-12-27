<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Advanced Notes App</title>
    <style>
      * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
      }

      body {
        font-family: Arial, sans-serif;
        line-height: 1.6;
        padding: 20px;
        background-color: #f5f5f5;
      }

      .container {
        max-width: 1200px;
        margin: 0 auto;
        display: grid;
        grid-template-columns: 2fr 1fr;
        gap: 20px;
      }

      .editor-section {
        background: white;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      }

      .editor {
        border: 1px solid #ccc;
        border-radius: 4px;
        margin-bottom: 15px;
      }

      .toolbar {
        display: flex;
        gap: 10px;
        padding: 10px;
        border-bottom: 1px solid #eee;
        flex-wrap: wrap;
        background: #f8f8f8;
      }

      .toolbar button,
      .toolbar select {
        padding: 6px 12px;
        cursor: pointer;
        border: 1px solid #ddd;
        border-radius: 4px;
        background: white;
        transition: all 0.3s ease;
      }

      .toolbar button:hover {
        background: #f0f0f0;
      }

      .content-area {
        min-height: 200px;
        padding: 15px;
        outline: none;
      }

      .note-title {
        width: 100%;
        padding: 10px;
        margin-bottom: 15px;
        border: 1px solid #ccc;
        border-radius: 4px;
        font-size: 16px;
      }

      .save-btn {
        background: #4caf50;
        color: white;
        padding: 10px 20px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-size: 16px;
        transition: background 0.3s ease;
      }

      .save-btn:hover {
        background: #45a049;
      }

      .notes-list {
        background: white;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      }

      .note-card {
        border: 1px solid #eee;
        border-radius: 4px;
        padding: 15px;
        margin-bottom: 15px;
        transition: all 0.3s ease;
        cursor: pointer;
      }

      .note-card:hover {
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
      }

      .note-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 10px;
      }

      .note-actions {
        display: flex;
        gap: 8px;
      }

      .note-actions button {
        padding: 5px 8px;
        cursor: pointer;
        border: none;
        background: none;
        font-size: 16px;
        transition: transform 0.2s ease;
      }

      .note-actions button:hover {
        transform: scale(1.1);
      }

      .pinned {
        border-left: 3px solid #4caf50;
        background: #f8fff8;
      }

      .note-content {
        padding: 5px;
        word-wrap: break-word;
      }

      .align-group {
        display: flex;
        gap: 5px;
        border-left: 1px solid #ddd;
        padding-left: 10px;
      }

      @media (max-width: 768px) {
        .container {
          grid-template-columns: 1fr;
        }
        .notes-list {
          margin-top: 20px;
        }
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="editor-section">
        <input
          type="text"
          id="noteTitle"
          class="note-title"
          placeholder="Note title"
        />
        <div class="editor">
          <div class="toolbar">
            <button onclick="formatText('bold')" title="Bold">B</button>
            <button onclick="formatText('italic')" title="Italic">
              <i>I</i>
            </button>
            <button onclick="formatText('underline')" title="Underline">
              <u>U</u>
            </button>

            <select
              onchange="formatText('fontSize', this.value)"
              title="Font Size"
            >
              <option value="3">Normal</option>
              <option value="1">Small</option>
              <option value="5">Large</option>
              <option value="7">Huge</option>
            </select>

            <div class="align-group">
              <button onclick="formatText('justifyLeft')" title="Align Left">
                ⫷
              </button>
              <button onclick="formatText('justifyCenter')" title="Center">
                ≣
              </button>
              <button onclick="formatText('justifyRight')" title="Align Right">
                ⫸
              </button>
            </div>
          </div>
          <div id="editor" class="content-area" contenteditable="true"></div>
        </div>
        <button onclick="saveNote()" class="save-btn">Save Note</button>
      </div>
      <div class="notes-list">
        <h2>My Notes</h2>
        <div id="notesList"></div>
      </div>
    </div>

    <script>
      let notes = [];
      const editor = document.getElementById("editor");
      const titleInput = document.getElementById("noteTitle");
      let currentNoteId = null;

      // Load notes from localStorage
      function loadNotes() {
        const savedNotes = localStorage.getItem("notes");
        if (savedNotes) {
          notes = JSON.parse(savedNotes);
          renderNotes();
        }
      }

      // Format text in editor
      function formatText(command, value = null) {
        document.execCommand(command, false, value);
        editor.focus();
      }

      // Save note
      function saveNote() {
        const title = titleInput.value.trim();
        const content = editor.innerHTML;

        if (!title) {
          alert("Please enter a title");
          return;
        }

        if (currentNoteId) {
          // Update existing note
          const index = notes.findIndex((note) => note.id === currentNoteId);
          if (index !== -1) {
            notes[index] = {
              ...notes[index],
              title,
              content,
            };
          }
        } else {
          // Create new note
          const newNote = {
            id: Date.now(),
            title,
            content,
            pinned: false,
            encrypted: false,
          };
          notes.push(newNote);
        }

        localStorage.setItem("notes", JSON.stringify(notes));
        resetEditor();
        renderNotes();
      }

      // Reset editor
      function resetEditor() {
        titleInput.value = "";
        editor.innerHTML = "";
        currentNoteId = null;
      }

      // Edit note
      function editNote(id) {
        const note = notes.find((note) => note.id === id);
        if (note && !note.encrypted) {
          currentNoteId = note.id;
          titleInput.value = note.title;
          editor.innerHTML = note.content;
          titleInput.focus();
        } else if (note.encrypted) {
          alert("Please decrypt the note first to edit it.");
        }
      }

      // Pin/Unpin note
      function togglePin(id) {
        const index = notes.findIndex((note) => note.id === id);
        if (index !== -1) {
          notes[index].pinned = !notes[index].pinned;
          localStorage.setItem("notes", JSON.stringify(notes));
          renderNotes();
        }
      }

      // Delete note
      function deleteNote(id) {
        if (confirm("Are you sure you want to delete this note?")) {
          notes = notes.filter((note) => note.id !== id);
          localStorage.setItem("notes", JSON.stringify(notes));
          renderNotes();
        }
      }

      // Encrypt note
      function encryptNote(id) {
        const password = prompt("Enter password for encryption:");
        if (!password) return;

        const index = notes.findIndex((note) => note.id === id);
        if (index !== -1) {
          notes[index] = {
            ...notes[index],
            encrypted: true,
            password: password,
            encryptedContent: notes[index].content,
            content: "🔒 Encrypted Content",
          };
          localStorage.setItem("notes", JSON.stringify(notes));
          renderNotes();
        }
      }

      // Decrypt note
      function decryptNote(id) {
        const note = notes.find((note) => note.id === id);
        if (!note || !note.encrypted) return;

        const password = prompt("Enter password to decrypt:");
        if (password === note.password) {
          note.content = note.encryptedContent;
          note.encrypted = false;
          delete note.password;
          delete note.encryptedContent;
          localStorage.setItem("notes", JSON.stringify(notes));
          renderNotes();
        } else {
          alert("Incorrect password!");
        }
      }

      // Render notes list
      function renderNotes() {
        const notesList = document.getElementById("notesList");
        const sortedNotes = [...notes].sort((a, b) => {
          if (a.pinned && !b.pinned) return -1;
          if (!a.pinned && b.pinned) return 1;
          return b.id - a.id;
        });

        notesList.innerHTML = sortedNotes
          .map(
            (note) => `
                <div class="note-card ${note.pinned ? "pinned" : ""}">
                    <div class="note-header">
                        <h3>${note.title}</h3>
                        <div class="note-actions">
                            <button onclick="editNote(${
                              note.id
                            })" title="Edit">✏️</button>
                            <button onclick="togglePin(${note.id})" title="${
              note.pinned ? "Unpin" : "Pin"
            }">${note.pinned ? "📌" : "📍"}</button>
                            <button onclick="deleteNote(${
                              note.id
                            })" title="Delete">🗑️</button>
                            <button onclick="${
                              note.encrypted ? "decryptNote" : "encryptNote"
                            }(${note.id})" title="${
              note.encrypted ? "Decrypt" : "Encrypt"
            }">
                                ${note.encrypted ? "🔓" : "🔒"}
                            </button>
                        </div>
                    </div>
                    <div class="note-content" onclick="editNote(${note.id})">
                        ${note.content}
                    </div>
                </div>
            `
          )
          .join("");
      }

      // Initialize app
      loadNotes();
    </script>
  </body>
</html>
