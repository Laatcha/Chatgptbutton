# Chatgptbutton
Button to upload files to chat gpt. From: https://www.youtube.com/@automatemylife00

"Generate a Js script that creates a button with the text ‘Submit File’ and inserts it into the DOM before an element with the class ‘.flex.flex-col.w-full.py-2.flex-grow.md\:py-3.md\:pl-4’. The button should have a green background color, white text color, 5px padding, no border, 5px border radius and 5px margin. The script should also create a progress element and insert it into the DOM before the same element. The progress element should have a width of 99%, height of 5px and a grey background color. Inside the progress element, there should be another div element representing the progress bar with a width of 0%, height of 100% and blue background color. When the button is clicked, it should create an input element of type ‘file’ that accepts ‘.txt’, ‘.js’, ‘.py’, ‘.html’, ‘.css’, ‘.json’ and ‘.csv’ files. Once a file is selected,  using an async it should be read as text and split into chunks of size 15000. using async Each chunk should be submitted into a conversation by doing the following:
 async function submitConversation(text, part, filename) {
        const textarea = document.querySelector("textarea[tabindex='0']");
        const enterKeyEvent = new KeyboardEvent("keydown", {
          bubbles: true,
          cancelable: true,
          keyCode: 13,
        });
        textarea.value = `Part ${part} of ${filename}: \n\n ${text}`;
        textarea.dispatchEvent(enterKeyEvent);
      }. 
The progress bar should be updated after each chunk is submitted within the for loop as follows
progressBar.style.width = `${((i + 1) / numChunks) * 100}%`;
 and should also check if chatgpt is ready with this code:
chatgptReady = false;
      while (!chatgptReady) {
        await new Promise((resolve) =＞ setTimeout(resolve, 1000));
        chatgptReady = !document.querySelector(
          ".text-2xl ＞ span:not(.invisible)"
Once all chunks have been submitted, the progress bar should turn blue.

# Response
// Create button
const button = document.createElement('button');
button.innerText = 'Submit File';
button.style.backgroundColor = 'green';
button.style.color = 'white';
button.style.padding = '5px';
button.style.border = 'none';
button.style.borderRadius = '5px';
button.style.margin = '5px';

// Create progress element
const progress = document.createElement('progress');
progress.style.width = '99%';
progress.style.height = '5px';
progress.style.backgroundColor = 'grey';

// Create progress bar inside progress element
const progressBar = document.createElement('div');
progressBar.style.width = '0%';
progressBar.style.height = '100%';
progressBar.style.backgroundColor = 'blue';

// Append button and progress elements before the specified element
const targetElement = document.querySelector('.flex.flex-col.w-full.py-2.flex-grow.md\\:py-3.md\\:pl-4');
targetElement.parentNode.insertBefore(button, targetElement);
targetElement.parentNode.insertBefore(progress, targetElement);

// Button click event handler
button.addEventListener('click', async () => {
  const fileInput = document.createElement('input');
  fileInput.type = 'file';
  fileInput.accept = '.txt, .js, .py, .html, .css, .json, .csv';

  fileInput.addEventListener('change', async (event) => {
    const file = event.target.files[0];
    const reader = new FileReader();

    // Read file as text
    reader.readAsText(file);

    reader.onload = async () => {
      const fileText = reader.result;
      const chunkSize = 15000;
      const numChunks = Math.ceil(fileText.length / chunkSize);

      for (let i = 0; i < numChunks; i++) {
        const start = i * chunkSize;
        const end = start + chunkSize;
        const chunk = fileText.substring(start, end);

        await submitConversation(chunk, i + 1, file.name);

        progressBar.style.width = `${((i + 1) / numChunks) * 100}%`;
      }

      progressBar.style.backgroundColor = 'blue';
    };
  });

  fileInput.click();
});

// Function to submit conversation
async function submitConversation(text, part, filename) {
  const textarea = document.querySelector("textarea[tabindex='0']");
  const enterKeyEvent = new KeyboardEvent('keydown', {
    bubbles: true,
    cancelable: true,
    keyCode: 13,
  });

  textarea.value = `Part ${part} of ${filename}:\n\n${text}`;
  textarea.dispatchEvent(enterKeyEvent);

  // Check if chatgpt is ready
  let chatgptReady = false;
  while (!chatgptReady) {
    await new Promise((resolve) => setTimeout(resolve, 1000));
    chatgptReady = !document.querySelector('.text-2xl > span:not(.invisible)');
  }
}
