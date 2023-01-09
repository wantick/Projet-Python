def decoupIp(adrIp):
    """ Fonction qui à pour fonctionalité de transfomer une chaine de caratères en liste en prenant comme délimiteur le "." 
    paramètre :
                adrIp est la chaine de caractère qui va être transforée en liste.
                 """
    decoup = adrIp.split(".")
    return decoup

def adressePrHôte(adrIp):
    """ Fonction qui à pour fonctionalité de retourner l'addresse Ip du premier hôte du sous-réseau 
    paramètre :
                adrIp est une liste de l'IP du sous-réseau.
                 """
    adrIp[3] = str(int(adrIp[3]) + 1) 
    Ip = ".".join(adrIp)
    return Ip

def adresseDrHôte(adrIp,nbr):
    """ Fonction qui à pour fonctionalité de retourner l'addresse Ip du dernier hôte du sous-réseau 
    paramètre :
                adrIp est une liste de l'IP du sous-réseau.
                nbr est le nombre d'hôtes demandé. 
                 """
    adrIp[3] = str(int(adrIp[3]) + (nbr - 1 ))
    Ip = ".".join(adrIp)
    return Ip

def adrDiffusion(adrIp):
    """ Fonction qui à pour fonctionalité de retourner l'addresse Ip de diffusion du sous-réseau 
    paramètre :
                adrIp est une liste de l'IP du sous-réseau.
                nbr est le nombre d'hôtes demandé. 
                 """
    adrIp[3] = str(int(adrIp[3]) + 1)
    Ip = ".".join(adrIp)
    return Ip

def adrReseau(adrIp,nbr,cpt):
    """ Fonction qui à pour fonctionalité de retourner l'addresse du sous-réseau 
    paramètre :
                adrIp est une liste de l'IP du sous-réseau.
                nbr est le nombre d'hôtes demandé. 
     """
    adrIp[3]= str(0)            
    adrIp[3] = str(int(adrIp[3]) + (nbr + 2) * cpt)
    adrReseau = ".".join(adrIp)
    return adrReseau

def nombreSousReseaux(nbr):
    """ Fonction qui à pour fonctionalité de trouver le bon nombre de sous-réseaux par raport au puissance de 2 
    paramètre :
                nbr est le nombre de sous-réseaux demandés."""
    reseau = 2
    cpt = 1
    while nbr >= reseau :
        cpt += 1
        reseau = pow(2,cpt)
    return reseau 

def nbrSousReseau2(nbr):
    """ Fonction qui à pour fonctionalité de trouver le bon nombre de sous-réseaux grâce au nombre d'hôtes demandés
    paramètre :
                nbr est le nombre d'hôtes demandé."""
    octet = 255
    hôtes = octet / ( nbr + 2 )
    hôtes = round(hôtes)
    return hôtes
    
def nombreHôtes(nbr):
    """ Fonction qui à pour fonctionalité de trouver le bon nombre d'hôtes possible par raport à la forme 2^^n -2 
    paramètre :
                nbr est le nombre d'hôtes demandés."""
    user = 2
    cpt = 1
    while nbr > user :
        cpt += 1
        user = (pow(2,cpt))-2
    return user

def nombreHôtes2(nbr):
    """ Fonction qui à pour fonctionalité de trouver le bon nombre d'hôtes possible par raport au nombre de sous-réseaux demandé
    paramètre :
                nbr est le nombre de sous-réseaux demandés."""
    octet = 255
    hôtes = round(octet / nbr )-2
    return hôtes  

def newMasqueReseau(masque,nbr):
    """ Fonction qui à pour fonctionalité de trouver Nouveau masque réseau 
    paramètre :
                nbr est le nombre d'hôtes demandés."""
    for cpt in range(0,4):
        if masque[cpt] == "0":
            octet = 254-nbr
            masque[cpt]=str(octet)
            masques = ".".join(masque)
            return masques
        
        elif cpt == 3 : 
            octet = 254-nbr
            masque[3]=str(octet)
            masques = ".".join(masque)
            return masques 

def CIDR(cidr):
    """ Fonction qui à pour fonctionalité de transformer un masque sous forme CIDR en masque en octet  
    paramètre :
                cidr est le masque à la forme CIDR."""
    octet = ["0","0","0","0","0","0","0","0"]
    cpt2 = 0
    masque = []
    cidr = int(cidr[1:3])
    for cpt in range (0,4):
        
        if cidr >= 8 :
            cidr -=  8
            masque.append("255")
       
        elif cidr < 8 :
            octet = ["0","0","0","0","0","0","0","0"]
            while cidr < 8 and cidr >=1 :
                cidr -= 1
                octet[cpt2] = "1"
                cpt2 += 1
            octet = "".join(octet)
            octet =int(octet,2)
            masque.append(str(octet))
        elif cidr == 0 :
            masque.append("0")

    masque = ".".join(masque)
    return masque

def CIDR2(masque):
    """ Fonction qui à pour fonctionalité de transformer un masque sous forme d'octet en un masque sous forme CIDR. 
    paramètre :
                masque est le masque à la forme d'octet."""
    cidr = ["1","1","1","1","1","1","1","1","1","1","1","1","1","1","1","1","1","1","1","1","1","1","1","1",]
    masque = masque.split(".")
    valeurCidr = 0 
    octet = masque[3]
    octet = bin(int(octet))
    octet = octet[2::]
    for cpt in range(len(octet)):
        cidr.append(octet[cpt])
    while len(cidr) != 0 :
        bit = cidr[0]
        cidr.pop(0)
        if  bit == "1" :
            valeurCidr += 1 
    resultat = "/" + str(valeurCidr) 
    return resultat

# corp du code 
nbrUser = 0
nbrRéseau = 0
choix  = "False"
Ip = input("Quelle adresse? ")
masque = input("Quel masque? ")


# si le masque est sous la forme CIDR 
if masque[0] == "/": 
    masque = CIDR(masque)


#menu 
while choix == "False": 
    
    menu = int(input("Définir le nombre d'hôtes (1) ou de sous-réseaux (2) ? "))
    
    # si l'utilistateur choisi la division par nombre d'hôtes
    if menu == 1 : 
        nbrUser = int(input("Nombre d'hôtes ? "))
        nbrUser = nombreHôtes(nbrUser)
        nbrRéseau = nbrSousReseau2(nbrUser)
        choix  = "True"
    
    # si l'utilistateur choisi la division par nombre de sous-réseaux
    elif menu == 2 :  
        nbrRéseau = int(input("Nombre de sous-réseaux ? "))
        nbrRéseau = nombreSousReseaux(nbrRéseau)
        nbrUser = nombreHôtes2(nbrRéseau)
        choix  = "True"
    
    # si l'utilistateur choisi un numéro qui n'est pas dans le menu
    else : 
        print("Le chiffre que vous avez tapez n'est pas dans le menu veuillez réessayer")
        choix  = "False"

# affichage des sous-réseaux
masque = decoupIp(masque)
MasqueReseau = newMasqueReseau(masque,nbrUser)
cidrMasqueReseau = CIDR2(MasqueReseau)
print(" ")
print("Nouveau masque réseau:",MasqueReseau,"(",cidrMasqueReseau,")")
print("Nombre de sous-réseaux",nbrRéseau)
print(" ")

#affichage du premier sous-réseau 
print("Résqeau n°1")
print("Masque réseau:",MasqueReseau)
print("Addresse réseau:",Ip) 

adrIp =  decoupIp(Ip) 
prAdr = adressePrHôte(adrIp)
drAdr = adresseDrHôte(adrIp,nbrUser)
print("Adresse du premier hôte: ",prAdr)
print("Adresse du dernier hôte: ",drAdr)
adrDif = adrDiffusion(adrIp)
print("Adresse de diffusion: ",adrDif)
print("Nombre maximal d'hôtes: ",nbrUser)
    
addrReseau = adrIp 
addrReseau[3]= str(nbrUser+2)
addrReseau = ".".join(addrReseau)

#affichage des sous-réseaux suivant 
for cpt in range(2,nbrRéseau+1):
    adrIp =  decoupIp(addrReseau)
    prAdr = adressePrHôte(adrIp)
    drAdr = adresseDrHôte(adrIp,nbrUser)
    adrDif = adrDiffusion(adrIp)
           
    print(" ")
    print("Réseau n°",cpt)
    print("Masque réseau:",MasqueReseau)
    print("Addresse réseau: ",addrReseau)
    print("Adresse du premier hôte: ",prAdr)
    print("Adresse du dernier hôte: ",drAdr)
    print("Adresse de diffusion: ",adrDif)
    print("Nombre maximal d'hôtes: ",nbrUser)    

    addrReseau = adrReseau(adrIp,nbrUser,cpt)
