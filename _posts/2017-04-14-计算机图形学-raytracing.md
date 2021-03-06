# 计算机图形学课程展示
## Assignment III: Ray Tracing
  首先需要先设置光的参数和物体的材质：
  
        // ------------------------------------------- 
          // Lighting parameters:
     
          GLfloat light_pos[] = {0.0f, 5.0f, 5.0f, 1.0f};
          GLfloat light_Ka[]  = {1.0f, 0.5f, 0.5f, 1.0f};
          GLfloat light_Kd[]  = {1.0f, 0.1f, 0.1f, 1.0f};
          GLfloat light_Ks[]  = {1.0f, 1.0f, 1.0f, 1.0f};
     
          glLightfv(GL_LIGHT0, GL_POSITION, light_pos);
          glLightfv(GL_LIGHT0, GL_AMBIENT, light_Ka);
          glLightfv(GL_LIGHT0, GL_DIFFUSE, light_Kd);
          glLightfv(GL_LIGHT0, GL_SPECULAR, light_Ks);
     
          // -------------------------------------------
          // Material parameters:
     
          GLfloat material_Ka[] = {0.5f, 0.0f, 0.0f, 1.0f};
          GLfloat material_Kd[] = {0.4f, 0.4f, 0.5f, 1.0f};
          GLfloat material_Ks[] = {0.8f, 0.8f, 0.0f, 1.0f};
          GLfloat material_Ke[] = {0.1f, 0.0f, 0.0f, 0.0f};
          GLfloat material_Se = 20.0f;
     
          glMaterialfv(GL_FRONT_AND_BACK, GL_AMBIENT, material_Ka);
          glMaterialfv(GL_FRONT_AND_BACK, GL_DIFFUSE, material_Kd);
          glMaterialfv(GL_FRONT_AND_BACK, GL_SPECULAR, material_Ks);
          glMaterialfv(GL_FRONT_AND_BACK, GL_EMISSION, material_Ke);
          glMaterialf(GL_FRONT_AND_BACK, GL_SHININESS, material_Se);
     
  本章的重点为shader着色器，本次我的实验中使用了两种模型，分别为phong shading,lambert shading.
  
phong shading模型：考虑环境光，漫反射和镜面反射的模型，三种模型的计算公式在下列的fragment shader和vertex shader中给出。

Lambert shading模型：此种模型下只考虑漫反射。计算公式也在下列的shader文件中给出。
  
 phong：
 
      vec4 Iamb = gl_FrontLightProduct[0].ambient;

     //calculate Diffuse Term:  
 
     vec4 Idiff = gl_FrontLightProduct[0].diffuse * max(dot(N,L), 0.0);   
 
     // calculate Specular Term:
 
     vec4 Ispec = gl_FrontLightProduct[0].specular * pow(max(dot(R,E),0.0),0.3*gl_FrontMaterial.shininess);
 
     // write Total Color:  
 
     gl_FragColor = gl_FrontLightModelProduct.sceneColor + Iamb + Idiff + Ispec;   `

  
  lambert:
  
       vec3 normal, lightDir;  
       
       vec4 diffuse;  
       
       float NdotL;  
       
       /* 法线向量 */  
       
      normal = normalize(gl_NormalMatrix * gl_Normal); 
      
      /* 入射光向量*/  
      
      lightDir = normalize(vec3(gl_LightSource[0].position));  
      
 	   /* cosθ */  
     
 	   NdotL = max(dot(normal, lightDir), 0.0);
 	   /* 散射项 */
     
 	   diffuse = gl_FrontMaterial.diffuse * gl_LightSource[0].diffuse;   
     
 	   gl_FrontColor = NdotL * diffuse;gl_Position = ftransform();  
     
 


## 以下为效果展示：

![Lambert](/images/blog/diffuse.gif)
![phong](/images/blog/phong.gif)
