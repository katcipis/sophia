​[1:10] 
for start in _frame_delimiters:
      for end in _frame_delimiters:
          #If you think pointers are horrible, references are worse...at least
          #pointers are explicit about what is happening
          s = start
          e = end
          funcs.append(lambda partners_info: _frame_by(partners_info, s, e))

a cada iteracao do loop
eu altero o lambda previamente criado

​[1:10] 
esse for ae nao funcionou, apesar de lexicamente eu estar criando variaveis novas, s e

​[1:10] 
for start in _frame_delimiters:
     for end in _frame_delimiters:
         #If you think pointers are horrible, references are worse...at least
         #pointers are explicit about what is happening
         funcs.append(lambda partners_info: _frame_by(partners_info, start, end))

​[1:11] 
tambem não funciona

​[1:11] 
no final todos os lambdas acabam apontando para o mesmo start/end....em vez de a combinacao completa

​[1:11] 
ai tentei

​[1:11] 
for i, _ in enumerate(_frame_delimiters):
       for j, _ in enumerate(_frame_delimiters):
           #If you think pointers are horrible, references are worse...at least
           #pointers are explicit about what is happening
           start = _frame_delimiters[i]
           end = _frame_delimiters[j]
           funcs.append(lambda partners_info: _frame_by(partners_info, start, end))

​[1:11] 
Tambem não funciona

​[1:12] 
no final todos os lambdas compartilham o mesmo closure...em vez de cada um ter o seu proprio start / end

​[1:12] 
o que vcs acham...to enlouquecendo ou o suporte a closure do python eh um lixo ?

​[1:12] 
serio...em lua...ou go...ja teria resolvido isso de primeira...ao invés disso to numa sofrência do cao

​[1:12] 
soh quero criar uma lista de funcoes....pqp

​[1:13] 
invoco @i4k para a treta tb

​[1:14] 
se eu tiver que criar uma classe com um metodo __call__ para resolver isso....desisto do python
