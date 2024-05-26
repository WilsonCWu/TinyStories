# TinyStories
https://arxiv.org/pdf/2305.07759

##  20240525
prelim: 
* good data for training new model. don't see much about the model architecture other than hyperparameters, i guess this means i should use standard architecture
* Two datasets: tinystories and tinystories-instruct. looks like they're each meant to train a different model.
* tinystories easier to start with
* meant to run on a single machine.
trying it out
https://github.com/WilsonCWu/TinyStories/blob/main/explore.ipynb

##  20240526
model sizing
* `We use GPT-Neo architecture with window size 256 and context length 512.` ??? arent those the same thing
* `Our results, shown in Figure 24, suggest that in the regime where the number of heads is small, increasing it improves the performance of the model across all metrics.` `To understand the model’s attention pattern after training, we use a 1-layer model with hidden dimension 1024 and 16 attention heads that was trained on TinyStories` ok let's start with head size 64

Try training
* https://github.com/WilsonCWu/nanoGPT/tree/tinystories using this because it's relatively simple and we're training a small thing
```
# total batch size ~0.5M??? assuming that's a good number based on train_gpt2
gradient_accumulation_steps = 30 
batch_size = 64
block_size = 256 # context of up to 256 previous characters

# baby GPT model :)
n_layer = 6
n_head = 6
n_embd = 384
dropout = 0.2

learning_rate = 1e-3 # with baby networks can afford to go a bit higher
max_iters = 5000
lr_decay_iters = 5000 # make equal to max_iters usually
min_lr = 1e-4 # learning_rate / 10 usually
beta2 = 0.99 # make a bit bigger because number of tokens per iter is small

warmup_iters = 100 # not super necessary potentially
```
```
tokens per iteration will be: 491,520
Initializing a new model from scratch
defaulting to vocab_size of GPT-2 to 50304 (50257 rounded up for efficiency)
number of parameters: 29.94M
num decayed parameter tensors: 26, with 30,031,872 parameters
num non-decayed parameter tensors: 13, with 4,992 parameters
using fused AdamW: True
```
oops i labelled this as 10M but it is actually 30M params
https://wandb.ai/pronto-studios/tinystories/runs/biudrcml?nw=nwuserwilsoncwu

* 5000 steps, ~1.5h A100
* step 5000: train loss 1.4951, val loss 1.5131
```
/content/nanoGPT# python sample.py --out_dir=out-tinystories
Overriding: out_dir = out-tinystories
number of parameters: 29.94M
No meta.pkl found, assuming GPT-2 encodings...
<|endoftext|>Once upon a time, there was a little girl named Lily. She loved to play dress-up and wear pretty jewelry. One day, she found an unusual necklace on the ground. It was long and pretty. She picked it up and put it on her wrist.

Later that day, Lily and her mom went to the store. Lily's mom said she needed to pay for the silver necklace. Lily didn't want to pay for the necklace, but she knew she had to. She asked her mom if she could buy the necklace, but her mom said no.

Lily was sad but then she remembered her bracelet necklace. She went home and put it on her necklace. She felt happy and loved. The end.<|endoftext|>Once upon a time, there was a little girl named Lily. She loved to play with her toys and sing songs. One day, her mom told her they were going to the park. Lily was so excited!

At the park, there was a big fire in the park. Lily's mom told her to be careful because the fire was very hot. Lily listened to her mom and waited for the fire to go away.

After the fire went away, Lily and her mom went to play on the slides. They went down the slide, went down very fast. Lily laughed and said, "That was so much fun!" And her mom agreed.<|endoftext|>Once upon a time, there was a little girl named Lily. She loved to play with her toys all day long. One day, her mom took her to a beach and they decided to play in the sand. They built a sandcastle and had so much fun.

After a while, Lily's mom said it was time to go home. Lily didn't want to leave. She said, "No, I want to stay and play more. I want to go to the beach and watch the sunset." But her mom said, "No, you can stay and play with your toys."

Lily was sad, but she understood that she could go to the beach later. As they walked home, Lily saw a big wave coming towards them and she got scared. But her mom told her not to worry and that they would be okay. Lily felt better and they went home together. The end.<|endoftext|>Once upon a time, there was a little girl named Lily. She had a toy that she loved very much. It was a charming doll that
---------------
<|endoftext|>Once upon a time, there was a little girl named Lily. She was very curious and loved to explore. One day, she went on a walk in the forest with her mommy and daddy. They saw a trail that they had never seen before. Lily wanted to follow the trail, but her mommy said it was too dangerous.

Lily didn't listen to her mommy and daddy and just started to wander across the trail. She was having so much fun! But then, she saw a big rock and tripped on it. She hurt herself badly and had to go to the hospital.

At the hospital, Lily learned that sometimes it's important to listen to grown-ups and not wander too far. She felt sad that she couldn't follow the trail, but she learned to be careful and not wander too far from home.<|endoftext|>Once upon a time, there was a little girl named Lily. She loved to play outside and explore the world around her. One day, she went on a walk with her mommy and saw a big tree with lots of branches. 

Lily wanted to climb the tree, but her mommy said, "Be careful, Lily. The branches are fragile and can break." Lily tried to climb the tree but it was too high. She felt uncomfortable and started to cry. 

Suddenly, Lily saw a bird flying by and she wanted to catch it. She grabbed a branch and tried to climb the tree, but fell down. Her mommy saw her crying and asked, "What's wrong, Lily?" Lily pointed to the bird and said, "The bird fell down. Look, it's a bird!" 

Her mommy helped her up and they continued to explore the world around them. They saw many new things and had lots of fun. The end.<|endoftext|>Once upon a time, there was a little bunny named Benny. Benny loved to hop around in the grass and eat carrots. One day, he found a carrot and he really wanted to eat it. 

Benny asked his friend, a little bird named Tweet, if she could lend him a carrot. Tweet said yes and gave Benny a carrot. Benny was so happy! 

But then, Benny realized he didn't have any carrots to eat. So, he went to his friend, the wise owl. The wise owl said, "Benny, I recommend you eat something yummy. Maybe
---------------
<|endoftext|>Once upon a time, there was a little boy named Timmy. Timmy had a toy car that he loved to play with. One day, Timmy's mom told him not to open his toy car because he might break it. Timmy was sad because he really wanted to play with his car.

But then Timmy's dad let him play with his car. Timmy was so happy! He played with his car all day long. When it was time for bed, Timmy put his toy car away and went to sleep. But he couldn't sleep because his car was gone. Timmy was sad because he wanted to play with his car again tomorrow.<|endoftext|>Once upon a time, there was a little girl named Lily. She had a charming teddy bear that she loved very much. One day, Lily's mom asked her to help her with the laundry. Lily didn't want to help because she was scared of the laundry. 

Her mom said, "Don't worry, I will provide you with your toys." Lily was happy and thanked her mom. She helped her mom put everything in the basket and got ready to go on a trip. 

On their trip, Lily's mom asked her to help bake some cookies with her. Lily was happy to help and they started the car. As they were driving down the aisle, Lily saw a toy that she liked most. She asked her mom if she could have it, but her mom said it was too expensive. 

Lily felt sad and didn't want to be ruined. But then, the car started to move and it was speeding down the street with them. Lily was so surprised and happy that she hugged the car tightly. The end.<|endoftext|>Once upon a time, there was a little girl named Lily. She loved to play outside in the rain. One day, she went to the river with her mommy and daddy. She saw a big boat come and she liked it a lot.

Lily wanted to go on the boat, but her mommy and daddy said, "No, it's too dangerous. You might get hurt." But Lily insisted she wanted to go on the boat. 

When they got to the boat, Lily's mommy and daddy told her they didn't have any toys to play with. Lily was sad, but she understood. She decided to play with her toys instead. She built a big castle and
---------------
<|endoftext|>Lily and Ben were playing in the park when they saw a big pile of leaves. They wanted to jump in the leaves and make a big pile.

"Let's go under the leaves!" Lily said. She was adventurous and curious.

"OK!" Ben said. He was brave and creative. He ran to the pile and climbed on the leaves. He reached for the leaves and pulled out some leaves.

"Yay!" he shouted. He hugged the pile and smiled.

Lily watched him and smiled too. She was curious and scared.

"Ben, it's okay!" she said. "You can do it! We can tie it together."

She bent down and wrapped the leaves around Ben's neck. They felt nice and snug.

They climbed down the pile and ran to their mom. She was waiting for them at the bottom of the hill.

"Mom, mom, can we go back to the park?" Ben asked.

"Sure, you can. But be careful. The leaves are very heavy and they can pick up easily," mom said.

Lily and Ben nodded and went back to the pile. They were happy and curious.<|endoftext|>Lily and Max were playing in the garden. They liked to dig in the dirt and look for bugs and worms. One day, they found a big worm with many legs and spots. They were very happy and picked it up.

"Look, I have a worm!" Lily said. "It is so pretty and soft."

"Wow, that is a nice worm!" Max said. "Can I see?"

"Sure, here you go." Lily gave him the worm. He tried to hold it in his hand, but he was too small. He tried to squeeze it, but he was too small.

"Let me try!" Max said. "I will squeeze the worm too."

Lily squeezed the worm gently. She felt the worm's warm skin. It moved its legs and legs. Max smiled and hugged the worm. He squeezed it gently.

"Can I have it?" Max asked.

"Sure, you can have it," Lily said. She reached for her hand.

Max reached out his hand. "Please, Lily, can I have the worm too?"

Lily looked at Max. He saw her hand and his
---------------
<|endoftext|>Once upon a time, there was a little girl named Lily. She had a toy that she loved to play with. One day, she went to the park with her mommy and daddy.

When they got to the park, they saw a man with a big sack. The man was carrying a heavy bag. Lily's mommy and daddy asked her if she wanted to help carry the bag. The man was so happy and said yes.

As they were walking around, Lily saw a man sitting on a bench. She asked him what was wrong, and he said he lost his wallet. Lily remembered her toy and wanted to help. She ran back to the man and gave him a big hug. The man was very grateful and thanked Lily for her help.

Lily learned that helping others can make you feel good inside. She also learned that helping others is important and can make someone happy.<|endoftext|>Once upon a time, there was a little girl named Lily. She loved to play with her dolls and teddy bears. One day, Lily's mommy told her they were going to visit her grandma who lived far away. Lily was so excited! She put on her favorite dress and jumped up and down, waving her arms in the air.

When they arrived at Grandma's house, Lily ran to open the door. She saw a big pile of clothes and was so happy. Her mommy said they were going to make a special dessert for her. Lily got out her favorite dress and some sprinkles.

When they got to grandma's house, they all had a delicious cake. Lily was so happy that she couldn't wait to share her birthday with her friends. She said, "I love making cake with you, grandma!"<|endoftext|>Once upon a time, there was a little boy named Timmy. Timmy loved to play outside, but one day he fell and hurt his knee. His mommy took him to the doctor and then he put a bandage on his knee. 

The doctor said Timmy had a boo-boo and needed to rest a lot. Timmy was scared of the pain, but his mommy held him and said it would feel better soon. 

After a few days of rest, Timmy felt better and his knee was all better. His mommy said, "Timmy, you are such a good boy. Does the pain go away now?" Timmy
---------------
<|endoftext|>Once upon a time there was a little boy named Tim. Tim was only three years old and he was always very curious. One day he was walking in the park when he noticed a big van parked by the road. Tim was so curious he decided to go take a closer look. As he got closer, he saw something move in the van. He asked the driver, "What is that?"

The driver said, "It's a delivery truck. You must run to the door and find out!"

Tim followed the driver as he opened the door. Inside he saw a delivery man with a big bag. The driver said, "Thank you so much, Tim! You are very kind." Tim smiled and waved goodbye to the delivery man as he left.

Tim couldn't wait to get home and tell his family about the delivery. He was so excited to show them the huge package he had been bought. He ran home to show them all the amazing things he had seen.<|endoftext|>Once there was a little girl, who was three years old. One day she was playing in her garden when she saw a bubble in the mud. She wondered who it belonged to.

She went to look for the bubble, but when she got there it was too deep. The girl was very sad and started to cry.

Suddenly her mommy appeared. She said to the girl, "It's okay. Let's go inside and find out what the bubble belongs to."

The little girl was so happy. She followed her mommy into the house.

They opened the door and inside the house there was a big, shiny bubble. It was smooth and sparkly and the girl was so delighted. She hugged the bubble and stayed with it for a while.

The little girl never wanted to leave the bubble, but she knew it was safe. She said, "Thank you Mommy and Daddy for the bubble, it was so shiny!"

The little girl smiled and said, "No problem, I'm glad you're happy!" She hugged her mommy and happily skipped back inside.<|endoftext|>Once upon a time, there was a little girl named Lucy. She was three years old and loved to explore. One day, she decided to go on an adventure. She put on her favourite colourful clothes and went outside to explore.

As she was walking, Lucy heard a noise. She stopped and listened. It sounded like
---------------
<|endoftext|>Once upon a time there was a little boy named Mark. He was very small and very adventurous. He loved exploring the world around him. One day, he was playing in the garden when he saw something bright and shiny. He couldn't believe his eyes! He reached out and touched it with his little hands.

Mark was so excited! He wanted to take it home with him. He started to dig around the garden, searching for something interesting. After a few minutes, he finally found a shiny object that had been hidden in the dirt. He was so excited!

Mark decided to keep the object safe. He would take it with him everywhere he went. He was so happy to have gained something so special and adventurous.

Mark kept the object with him wherever he went. He was always so excited to have found something new and exciting. And whenever he felt lonely, he would look up and smile.<|endoftext|>Once upon a time, there was a little girl named Lucy. She was three years old and always knew what to do.

One day, Lucy asked her mum for help. Her mum wanted to add a new wagon, so she got some tools and some nails. She went to the store to buy the wagon and she got to work.

But when she came back, her mum was not happy. She said, "Lucy, you are all too young to add a wagon. You need to go home and think about what you did."

Lucy felt sad and started to cry. But then she remembered she had a special tool - a hammer and a razor. She took the hammer and started to stir the new wagon. She was so excited and started making it into a smooth wagon.

The day came to an end and Lucy returned to her mum, happy to have her new wagon back. She gave her mum a big hug and said, "I'm so glad we added that new wagon!"<|endoftext|>Once upon a time, there was a little girl. She had a mommy and her dog. The little girl liked to play outside. Every day, her mommy and daddy would supply her with fresh, green grass to eat. 

One day while the mommy and daddy were out walking, the little girl spotted something furry in the grass. She ran over to it and kicked it with her foot. The furry thing got scared and jumped away. 

The little girl felt so brave
---------------
q<|endoftext|>Sara and Ben are friends. They like to play in the park. They see a big tree with red fruits. They want to pick some fruits.

"Look, Ben, look!" Sara says. "Let's pick some fruits and eat them!"

"OK, Sara, but be careful. The fruits are not good for them," Ben says. He is careful.

They run to the tree and pick some fruits. They are careful not to drop them. They hear a loud noise. They see a big dog. The dog is angry. He barks and growls.

"Go away, dog!" Sara says. "These are my fruits!"

"Go away, dog!" Ben says. "You are mean!"

The dog does not listen. He runs faster and faster. He does not see the man. The man is coming. He is angry too. He grabs a stick and a knife. He hits the dog on the face. The dog yelps and runs away.

"Help, help!" Sara and Ben say. They are scared. They scream. The man claps his hands. He catches the dog and puts him in a cage. He locks the cage.

Sara and Ben cry. They get in trouble. They cannot go home. They cry and hug each other.

The man does not know what to do. He calls for help. He is sad too. He has no kite. He has no parrot. He has nothing. He has no friend. He has no friend. He is alone.<|endoftext|>Tom and Sam are going to the airport with Mom and Dad. They are very happy. They have to pack their bags and get in the car. Mom and Dad tell Tom and Sam to buckle up their seat belts. They say they will stay in line until they are in the plane.

Tom and Sam open their seat belts and see many people with the plane. They see a man with a long beard and a long beard. He looks serious. Tom and Sam want to see the plane. They follow him.

"Can we go on the plane, Mom?" Tom asks.

"Sorry, we have to stay close. The plane is too high and too fast. We have to wait a little bit," Mom says.

Tom and Sam are sad. They do not like the plane. They want to
```
fixed start `"Two boys named Jake and Josh were playing in the forest.`
```
/content/nanoGPT# python sample.py --out_dir=out-tinystories
Overriding: out_dir = out-tinystories
number of parameters: 29.94M
No meta.pkl found, assuming GPT-2 encodings...
Two boys named Jake and Josh were playing in the forest. Jake wanted to show Josh something special. He said, "Let's go for a walk in the forest". 
Josh was curious and asked, "What do we do?". 
Jake replied, "Let's go for a walk and see what we can find".

So they headed out and soon found a big tree with big branches. They climbed up the tree and looked around. It was a beautiful sight.

"Wow, look at that tree! It's so big and tall!" said Jake.

The boys started to climb the tree. They walked and walked until they reached the top.

They looked around and saw lots of trees and flowers. Suddenly, they heard a loud noise. It was a big animal.

"What's that?" asked Jake. 

"That was a hawk!" said Josh, feeling very scared.

Jake and Josh looked at each other. They were very scared.

"What should we do?" asked Jake. 

"Let's go back down the tree," said Josh. 

So, the boys ran down the tree, back to the park, safe and sound.<|endoftext|>Once upon a time, there was a kind girl called Lucy. She had a pet rabbit named Fluffy. Fluffy had soft fur and Lucy loved him very much.

One day, Lucy decided to take Fluffy for a walk in the park. As she was walking, she saw a pile of powder all over the ground and decided to bring it closer to Fluffy.

When Lucy arrived, Fluffy was very happy to see Lucy. He hopped around the powder and Lucy laughed and clapped her hands. Lucy was so happy that Fluffy was safe.

After a while, Lucy decided it was time to go home. As she walked home, Fluffy waved goodbye to Lucy and gave her a big hug. Lucy smiled and waved goodbye as she walked away.

When Lucy got home, she was so tired but also very happy that she could bring Fluffy with her. The end.<|endoftext|>Once upon a time, there was a little boy named Timmy. Timmy loved to play outside in the sun. One day, while playing, Timmy fell down and hurt his knee. His mom took him to the hospital for a hospital.

At the hospital, they met a kind nurse named Mrs. Smith. Mrs. Smith looked very
---------------
Two boys named Jake and Josh were playing in the forest. Simon was very frightened, but Josh was very brave.

They started to climb the tree and over and over again. But then they started to quarrel. "I want to climb the tree," said Jake. 

"No, I want to climb the tree," said Josh. 

They argued and shouted until they finally decided to stop. 

"Let's do it together," shouted Jake. 

"Okay," said Josh. 

So they held hands and started to climb the tree. When they got to the top, they could see the whole forest. 

"Wow!" said Jake. "We did it!" 

They were both very happy that they could climb the tree together.<|endoftext|>Once upon a time, there was a family who was very happy. They had a secret. It was a big, shiny stone. The family wanted to make it even more special.

They went over to the house and found a big, red box. They put all their toys inside the box. Then they put all the toys in their places and made sure that the box was closed tight.

The family was so excited. They were very happy with their secret box. They knew it would be a great place to make everything happen. So they went outside and set the box on the grass.

The box made the toys look brand new. The family was excited and wanted to play with the toys. They put the toys in one pile and all the other toys in another.

The family was very happy with their secret box. They played and laughed, and made it even better. The family was so happy that they had found the perfect hiding place.<|endoftext|>Once upon a time, there was a little girl named Lily. She had a normal bedroom with a big bed and a cozy bed that she loved to sleep in. One day, Lily's mommy told her they were going to the park to play. Lily was so excited and said, "Yay, let's go!"

At the park, Lily saw a boy who was also playing with a ball. Lily's mommy said, "Would you like to play with the ball?" Lily said, "Yes, please!" And the boy threw the ball for her. Lily laughed and said, "That was fun! Can I have a turn?" Her mommy said, "Sure, you can have a turn.
---------------
Two boys named Jake and Josh were playing in the forest. Jake was wearing a yellow jacket and Josh was wearing a blue shirt.

"Hi Jake, do you want to play with me?" Josh asked.

"Yes please!" Jake replied. Jake put on his red jacket and Josh tried to guess what he was wearing.

Ryan said, "I'm wearing a blue shirt and a blue shirt. It's my favorite."

Jake smiled and said, "Yes, it's a suit too. Look at my blue shirt."

The boys started to play together and they laughed and laughed. Jake thought it was so much fun.

After a while, Jake got hungry. "I want to go get some food," he said.

Josh agreed and they shared a snack. Jake and Josh were very happy. They had a great day playing together and it was a perfect day.<|endoftext|>Once upon a time, there was a little boy named Jack. He was three years old and loved to explore. One day, he was walking in the woods when he saw a big, white rock. He was so excited, he wanted to climb it.

"Mum, can I climb that rock?" he asked. 

"Yes," replied his mum.

So Jack started to climb the rock. He was so busy trying to balance there! After a few minutes, he got tired and sat down to eat some grass.

After he finished eating, Jack continued on his walk. He saw a beautiful rainbow in the sky and he knew that he'd never forget it. He knew he would always remember this special day.<|endoftext|>Once upon a time, there was a little girl called Lucy. She loved to play outside in the sunshine.

One day, Lucy was playing in the garden. She found a stick. It was a big, white stick.

Lucy picked up the stick and showed it to her mum. "Mum, look what I found!" she said.

Mum smiled and said, "That's a very special stick, Lucy. It's a very special stick. It can use the stick to build a special castle."

Lucy was very excited. She wanted to make something special with the stick.

So she took the stick and started to make the castle. She used the stick to make a tall castle. When she was done, her mum said, "Now we have to go home."

---------------
Two boys named Jake and Josh were playing in the forest. Jake was looking for a place to hide. Suddenly, he saw a cobweb hanging from a tree. He wanted to be clever and use his finger to scare the spider away. Josh was worried and he quickly ran away.

Jake saw what Sam had done and he was very angry. He shouted at Josh that he must pay for the cobweb and shut the door. Josh felt bad for being so angry and he slowly opened the door.

The spider was still there, but Jake was also relieved. He said, "thank you for paying for the cobweb." Josh smiled and then used his finger to cut the cobweb from the tree.

The spider was very happy and thanked Jake for being so clever. The two boys went their separate ways and enjoyed the nice clean cobweb together.<|endoftext|>Once upon a time there was a little boy called Jack. He was three years old and he loved to explore. One day he went for a walk in the park. He saw something very shiny in the grass. It was a metal box.

Jack was very excited and he bent down to pick it up. He held it up and it was very interesting! He looked at it for a long time and then he noticed something strange. The metal box was filled with many tiny things like rocks and feathers.

Jack was so curious and he couldn't wait to know what they were. He carefully opened the box, and saw something amazing! There were two coloured birds and a little white deer inside! 

Jack was so happy he started to laugh. He had found something so special that he had found it all along. He decided to keep it forever and never forget the interesting things he found.<|endoftext|>Once upon a time, there was a little girl named Maria. She was feeling very adventurous and wanted to go on an adventure. She went to find her parents and she asked if they would take her on a magical journey. 

Her parents were very excited but also very careful as they drove to the airport. Maria was a bit scared, but she kept asking her parents questions and they said yes! 

When they arrived at the airport, Maria explored the airport. There were lots of people and there were many people. Maria started to feel scared, but her parents encouraged her to keep going.

They went across the street and went for a ride. Maria was so happy that she'd been on such an amazing journey!
---------------
Two boys named Jake and Josh were playing in the forest. Jake saw a strange creature with big eyes and a long tail. 

Ryan said, "What are you doing here? This is not a place for children. This is an axe."

Jake was scared but he said, "I'm just exploring."

The creature said, "Come, let's explore together."

Jake and Josh were so excited. They ran around and looked at the axe. They were amazed at how big it was. 

The creature said, "This is a very special axe. It makes us very brave."

Jake and Josh nodded. They decided to keep exploring the forest.<|endoftext|>Once upon a time there was a little girl named Amy. She was so excited because she was going to have a picnic outside her house.

Amy was so excited she ran to the kitchen and started to eat. But before she could eat, she saw a big crack in the wall. She looked up and said, "What's wrong?"

Suddenly, a voice answered, "I'm stuck in the wall. Can you help me?"

Amy looked up and saw a big, friendly mouse. The mouse said, "Yes, I can help you."

The mouse hopped up onto the wall and started to climb. With a mighty wave of its wings, Amy was able to get the mouse out.

The mouse thanked Amy for her help and started to cry. Soon the mouse was free, and Amy was full and happy. She thanked the mouse for helping her. From then on, Amy was always looking out for things to get her food.<|endoftext|>John was a little boy. He was three years old and he was playing in his room. Today he had a rag in his hand. It was a small rag and he was so excited. He wanted to move it around and play with it.

John said to himself, â€œI am going to play with it!â€ He grabbed the rag and started to pull it around. He rolled it and twisted it around with his feet. He laughed and laughed as he played.

John didnâ€™t want to move the rag. His mom saw him playing and said, â€œJohn, stop pulling the rag!â€ She took the rag away from John and said, â€œWhy donâ€™t you put the rag outside and keep playing? It
```
TODOs
* try smaller model size, paper used ~30h for model of this size, and probably trains at higher efficiency
* fix sample.py to terminate at <|endoftext|>
