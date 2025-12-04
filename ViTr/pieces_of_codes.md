# here you have all the blocks of code that are needed to complete the ViTr model, but in random order... so you read through them and dont just cut and paste the whole thing!

x = self.patch_embed(patches)  # (batch, num_patches, embed_dim)

model.build(input_shape=(None, 140, 170, 1))

patches = self.patch_extract(inputs)  # (batch, num_patches, patch_size*patch_size*3)

x_norm1 = self.norm1(x)

model = MinimalViT(patch_size=14, num_classes=num_classes)  # Use the actual num_classes (3)

 
self.norm1 = layers.LayerNormalization(epsilon=1e-6)
self.attn = layers.MultiHeadAttention(num_heads=4, key_dim=self.embed_dim // 4, dropout=0.1)
self.dropout_attn = layers.Dropout(0.1)
        
self.norm2 = layers.LayerNormalization(epsilon=1e-6)
self.mlp = keras.Sequential([
      layers.Dense(self.embed_dim * 2, activation='gelu'),
      layers.Dropout(0.1),
      layers.Dense(self.embed_dim),
      layers.Dropout(0.1),
])

history = model.fit(
    X_train, y_train,
    validation_split=0.2,
    epochs=10,
    batch_size=32
)


model.compile(
    optimizer='adam',
    loss='categorical_crossentropy',  # Use this for one-hot encoded y_train
    metrics=['accuracy']
)

x = x + self.pos_embed

attn_output = self.attn(query=x_norm1, key=x_norm1, value=x_norm1, training=training)
        attn_output = self.dropout_attn(attn_output, training=training)
        x = x + attn_output  # Add skip connection


# Layer Normalization 2 and MLP
        x_norm2 = self.norm2(x)
        mlp_output = self.mlp(x_norm2, training=training)
        x = x + mlp_output # Add skip connection
