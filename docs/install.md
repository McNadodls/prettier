import { checkButtonValidateImputs, enableValidation} from './validate.js';
import { initialCards } from './initialCards.js';
const ecsKey = 'Escape';
const popups = document.querySelectorAll('.popup');
const widowPopupProfile = document.querySelector('.popup_type_profile');
const editProfileBtn = document.querySelector('.button_do_profile-edit');

const formProfile = document.querySelector('.popup__form_type_profile');
const profileTitle = document.querySelector('.profile__title');
const profileSubtitle = document.querySelector('.profile__subtitle');
const popupInputName = document.querySelector('.popup__input_type_name');
const popupInputProfession = document.querySelector('.popup__input_type_profession');

const addElementBtn = document.querySelector('.button_do_profile-add');
const widowPopupCard = document.querySelector('.popup_type_card');
const elementTitle = document.querySelector('.popup__input_type_title'); 
const elementUrl = document.querySelector('.popup__input_type_url-img'); 

const elementsTemplate = document.querySelector('#elements-template'); 
const elements = document.querySelector('.elements');
const formCard = document.querySelector('.popup__form_type_card');

const widowPopupImage = document.querySelector('.popup_type_img');
const imagePopup = document.querySelector('.popup__image');
const signaturePopup = document.querySelector('.popup__signature');

const configForm = {
  formSelector: '.popup__form',
  inputSelector: '.popup__input',
  submitButtonSelector: '.button_type_send',
  inactiveButtonClass: 'buttont_type_disabled',
  inputErrorClass: 'popup__input_type_error',
  errorClass: 'popup__input-error_active'
}



function createNewCard(name, link) {
  const elementNewCard = elementsTemplate.content.cloneNode(true);
  const image = elementNewCard.querySelector('.element__image');
  const ImageLink = elementNewCard.querySelector('.buttont_type_like');
  const buttonDelete = elementNewCard.querySelector('.button_do_element-delete');
  elementNewCard.querySelector('.element__signature').textContent = name;
  image.src = link;
  image.alt = name;
  image.addEventListener('click', function () { 
    imagePopup.src = link;
    imagePopup.alt = name;
    signaturePopup.textContent = name;
    openPopup(widowPopupImage);
  });
  ImageLink.addEventListener('click', function () { 
    ImageLink.classList.toggle('buttont_type_like-active');
  });
  buttonDelete.addEventListener('click', function() {
    buttonDelete.closest('.element').remove();
   });
  return elementNewCard;
}

function addNewCard(newCard) { 
  elements.prepend(newCard);
}

function createInitialCard() {
  initialCards.forEach(function (elem) {
    const newCard = createNewCard(elem.name, elem.link);
    addNewCard(newCard);
  });
}

/*Профиль*/ 
function openPopupProfile() {/*открыть*/
  openPopup(widowPopupProfile);
  popupInputName.value = profileTitle.textContent;
  popupInputProfession.value = profileSubtitle.textContent;
}

function submitFormProfile(evt) {/*заменить*/
  evt.preventDefault();
  profileTitle.textContent = popupInputName.value;
  profileSubtitle.textContent = popupInputProfession.value;
  closePopup(widowPopupProfile);
}
/*карточки*/
function submitFormCard(evt) {
  evt.preventDefault();
  const newCard =  createNewCard(elementTitle.value, elementUrl.value);
  addNewCard(newCard);
  closePopup(widowPopupCard);
  formCard.reset();
  checkButtonValidateImputs(formCard, configForm);
}


function openPopup(popup) {
  popup.classList.add('popup_opened');
  document.addEventListener('keydown', checkKeyPressEsc);
}
function closePopup(popup) {
  popup.classList.remove('popup_opened');
  document.removeEventListener('keydown', checkKeyPressEsc);
}

function checkKeyPressEsc (evt) {
  if (evt.key === 'Escape') {
    closePopup(document.querySelector('.popup_opened'));
  }
  
}
createInitialCard();

editProfileBtn.addEventListener('click', openPopupProfile); //профиль
formProfile.addEventListener('submit', submitFormProfile);

addElementBtn.addEventListener('click', () => {openPopup(widowPopupCard)}); //карточка
formCard.addEventListener('submit', submitFormCard);

popups.forEach( (popup) => {
  popup.addEventListener('click', (evt) => {
     if ( evt.target.classList.contains('button_type_close') || evt.target.classList.contains('popup_opened') ) {
        closePopup(popup);
      } 
  });
});

enableValidation(configForm);
